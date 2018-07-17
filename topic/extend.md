!SLIDE center subsection blue

# Extend the DPK

!SLIDE bullets

# Build Your Environment

1. DPK is good at building demo environments
1. DPK uses Puppet, you can too!
1. Write DPK code to build your system

~~~SECTION:notes~~~
This is the best part of the DPK! It's a great tool out of the box, but we can make it do more. We can make the DPK build exactly the server we want, every time. No need to manually update files or forget to change configuration. Let Puppet do that for you.
~~~ENDSECTION~~~

!SLIDE bullets

# Learning Puppet

1. Puppet thinks in state, not procedures
1. Puppet uses types to represent state
1. Delivered Puppet types and DPK types
  1. `file{}`
  1. `file_line{}`
  1. `pt_webserver_domain{}`

~~~SECTION:notes~~~
We have full access to all the delivered Puppet types. And, all of the DPK types created by the PeopleTools team are available for us too.
~~~ENDSECTION~~~

!SLIDE bullets

# Puppet Resources

    @@@puppet
    type { 'title':
      attribute => value,
      attribute => value,
    }

File Resource

    @@@puppet
    file { 'healthcheck.html':
      path    => '/opt/www/',
      ensure  => present,
      content => "active",
    }

~~~SECTION:notes~~~
Puppet resources follow the same style. The `type`, followed the by `title` of the resource. The `title` must be unique in the catalog.

Depending on the `type`, different attribute are required/available to configure an item. In the example, we are declaring a `healthcheck.html`. It is located under `/opt/www` and contains the string `active`. If the file doesn't exist, Puppet will create it with that content. If the file exists, but the content doesn't match, Puppet will update the file to contain only the string `active`.
~~~ENDSECTION~~~

!SLIDE bullets

# Puppet Variables

1. Variable are immutable (no changes)
1. Variables are assigned when the catalog is built
1. Syntax: `$variable`
1. Variables can be interpolated: `"${variable}.psadmin.cloud"`

!SLIDE bullets

# Puppet Loops

1. For Each is used by the DPK
1. Loops through keys in a hash

        @@@puppet
        $var.each |$key, $value| {
          file { "${key}.html":
          path    => '/opt/www/',
          ensure  => present,
          content => "${value[subkey]}", 
        }

!SLIDE center subsection grey

# Demo

~~~SECTION:guide~~~
1. Build a manifest to deploy a Health Check file

> *Estimated Time: 30 min*

## Build a manifest to deploy a Health Check file

1. Create a new text file: `c:\psft\dpk\puppet\production\manifests\healtcheck.pp`

        @@@puppet
        $pia_domain_list = hiera('pia_domain_list')

        $pia_domain_list.each | $domain, $pia_domain_info | {
            $ps_cfg_home = $pia_domain_info['ps_cfg_home_dir']
            $portal_home = "${ps_cfg_home}/webserv/${domain}/applications/peoplesoft/PORTAL.war"

            file { "${domain}-healthcheck":
                ensure  => present,
                path    => "$portal_home/health.html",
                content => "true",
            }
        }

1. Run the manifest and test

        @@@powershellconsole
        PS C:\> cd \psft\dpk\puppet\production\manifests
        PS C:\psft\dpk\puppet\production\manifests> puppet apply .\healthcheck.pp -d --confdir=c:\psft\dpk\puppet

1. Verify the `health.html` file is built: `http://hr025-win-x.internal-x.aws.psadmin.cloud:8000/health.html`

~~~ENDSECTION~~~