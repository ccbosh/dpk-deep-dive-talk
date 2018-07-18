!SLIDE center subsection blue

# Puppet

!SLIDE bullets

# Puppet

* Popular configuration management tool
* Core technology behind the DPK
* Open source

!SLIDE bullets

# Roles and Profiles

The DPK follows the "Roles and Profiles" paradigm for writing Puppet code.

* Role == Server Function
* Roles combine Profiles
* Profiles == Technology
* Profiles read configuration data

~~~SECTION:notes~~~
For more on the Roles and Profiles paradigm, read this blog post: https://www.craigdunn.org/2012/05/239/
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Roles

A DPK Role defines the components/configuration to build. There are roles for:

* PeopleTools App, Web, Batch, AppBatch
* Application App, Web, Batch, AppBatch
* PeopleSoft Image Demo or PUM

To view a full list of roles, look under `c:\psft\dpk\puppet\production\modules\pt_role\manifests`

!SLIDE bullets

# DPK Roles

A DPK role is a collection of profiles. A profile is a set of reusable Puppet code.

For example, the `pt_app_midtier` DPK role (web, app, batch) includes these profiles:

    @@@puppet
    Class['::pt_profile::pt_app_deployment'] ->
    Class['::pt_profile::pt_tools_deployment'] ->
    Class['::pt_profile::pt_psft_environment'] ->
    Class['::pt_profile::pt_appserver'] ->
    Class['::pt_profile::pt_prcs'] ->
    Class['::pt_profile::pt_pia'] ->

~~~SECTION:notes~~~
If we looked at the `pt_tools_midtier` role, we would not see the `pt_app_deployment` profile. This DPK role will deploy the `PS_APP_HOME`, `PS_HOME`, install the middleware, and build web/app/batch domains.

We will revisit DPK roles later when we start customizing the DPK for our needs.
~~~ENDSECTION~~~

!SLIDE bullets

# site.pp

1. The DPK role is defined in `dpk\puppet\production\manifests\site.pp`
1. There is one `site.pp` file per server*
1. `site.pp` will call the code to configure your server

~~~SECTION:notes~~~
1. The rule for `site.pp` is not a hard rule. The Puppet Roles/Profiles paradigm that the DPK follows says that a server should only have one role.
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Role and Configuration Relationship

1. DPK Role controls what Puppet code is executed
1. DPK Configuration how the Puppet code builds the server

~~~SECTION:notes~~~
Keep this in mind as we go through the next sections: 

1. DPK Role == what puppet code executes
1. YAML Files == how the executing puppet code configures the server
~~~ENDSECTION~~~

!SLIDE bullets

# Running Puppet

Running `puppet apply` will:

1. Perform configuration lookups from YAML files and host data
1. Build a catalog of desired state
1. Apply the desired state to the server

!SLIDE bullets

# Idempotency

_Idempotence_: "operations that they can be applied multiple times without changing the result beyond the initial application"

1. Puppet supports idempotency
1. Run Puppet many times - will only make changes if there are changes to be made
1. DPK doesn't always respect idempotency:
  1. Tuxedo Domains
  1. PIA Domains
  1. ACM Plugins

~~~SECTION:notes~~~
The lack of idempotency in the DPK is the biggest problem with the DPK. By design, you can run Puppet every 30 minutes and it won't touch the server unless changes are needed.

With the DPK, every time you run Puppet the DPK code will shut down Tuxedo domains and PIA domains, configure the domains, and then boot the domains. This is mostly a function of how Tuxedo domains are configured via `psadmin`. (See the Ruby code for more details).
~~~ENDSECTION~~~

!SLIDE bullets

# Running Puppet

`puppet apply` command options

1. `--confdir=<path>`: where the Puppet configuration is stored (not the YAML files)
  1. In 8.55, this parameter is not required. 8.55 uses the default location.
  1. In 8.56, this parameter is required. 8.56 stores the configuration in `c:\psft\dpk\puppet`
1. `--debug` or `-d`: provide detailed Puppet output

!SLIDE bullets

# Running Puppet

1. `--execute` or `-e`: execute specific Puppet code
1. `--noop`: make no changes but report what Puppet would change
1. `--environment`: defaults to `production`, but Puppet can support multiple "environments". Environments are independent Puppet catalogs; you can have separate `dev` and `production` environments on the same machine.

~~~SECTION:notes~~~
The `-e` command is very useful to test parts of the DPK. I wish I would have know about this when I started. Running the entire `site.pp` manifest can take a while and slows down testing.
~~~ENDSECTION~~~

!SLIDE center subsection grey

# Demo

~~~SECTION:notes~~~
Demo: Look at the DPK Roles and Profiles
Create a 2nd app server
Run `puppet apply -e "include ::pt_profile::pt_appserver"`
Run `puppet apply -e "include ::pt_profile::pt_domain_boot"`
~~~ENDSECTION~~~