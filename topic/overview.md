!SLIDE bullets

# DPK Overview

!SLIDE bullets

# DPK Background

Deployment Packages are term that encompasses

1. Python Bootstrap Scripts
1. Software Archives
  1. WebLogic/Tuxedo/Java/Oracle
  1. PeopleTools
  1. PeopleSoft Application
1. Puppet
1. Puppet Modules

~~~SECTION:notes~~~
Generally speaking, when we talk about the DPK, we often refer to the Puppet Modules and code. But the DPK really includes all 3 pieces.
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Technologies

1. Puppet
1. Hiera
1. Facter
1. Python for bootstrapping 
  * (Powershell/Bash for 8.55)
1. Automated Configuration Management

~~~SECTION:notes~~~
In 8.56, the bootstrapping process was re-written in Python to make it cross platform.
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Types

1. PeopleTools DPK
1. Application DPK*
1. Elasticsearch DPK
1. COBOL DPK

~~~SECTION:notes~~~
There is no separate Application DPK download like the others. The PeopleSoft Images contain Application DPK, along with the PeopleTools DPK. To get an "Application DPK" (the `ps_app_home` tarball), you grab it from the PeopleSoft Image.
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Skillsets

1. Basic PS Admin Knowledge
1. Hiera, Facter, and YAML
1. Puppet
1. Scripting Experience
1. Ruby

~~~SECTION:notes~~~
The skills listed here are in order of importance. You HAVE to know basic PS Admin to do anything more than build PI's with the DPK. 

You won't need to know Ruby, but if you do you can really make the DPK work for you.
~~~ENDSECTION~~~


!SLIDE bullets

# DPK Considerations

1. Built for the PeopleSoft Images
1. Windows DPK sometimes behind in features
1. Not designed for maintenance*
1. Requires `root` access (not in 8.57+)

~~~SECTION:notes~~~
The DPK was built to spin up new environments on new servers, and it's really good at that. That's what "the cloud" needs.

The Windows DPK doesn't get the same love. Some features, like CPU patching via OPatch, have not been delivered for the Windows DPK.

The biggest limitation: the DPK is designed to build and has gaps when using it for maintenance. If you run `puppet apply`, the DPK will shut down every domain it knows about while it runs and then restart the domains. This means you cannot run `puppet apply` if users are on the system. There are ways to mitigate this behavior - we'll cover them - but it's a big issue.

Despite this, the DPK is a fantastic tool and still worth the investment.
~~~ENDSECTION~~~