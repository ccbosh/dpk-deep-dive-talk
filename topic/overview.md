!SLIDE center subsection blue

# DPK Overview

!SLIDE bullets

# Deployment Packages

* Delivered automation for building environments
* Define your environments upfront
* Implementation is abstracted
* Consistent and repeatable infrastructure
* Configuration is documented: `psft_customizations.yaml`
* Extend to customize your builds

!SLIDE bullets

# DPK Background

"Deployment Packages" is a term that encompasses:

* Python Bootstrap Scripts
* Software Archives
  * WebLogic/Tuxedo/Java/Oracle
  * PeopleTools
  * PeopleSoft Application
* Puppet
* Puppet Modules

~~~SECTION:notes~~~
Generally speaking, when we talk about the DPK, we often refer to the Puppet Modules and code. But the DPK really includes all 3 pieces.
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Technologies

*  Puppet
*  Hiera
*  Facter
*  Python for bootstrapping 
  * (Powershell/Bash for 8.55)
*  Automated Configuration Management

~~~SECTION:notes~~~
In 8.56, the bootstrapping process was re-written in Python to make it cross platform.
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Types

* PeopleTools DPK
* Application DPK*
* Elasticsearch DPK
* COBOL DPK

~~~SECTION:notes~~~
There is no separate Application DPK download like the others. The PeopleSoft Images contain Application DPK, along with the PeopleTools DPK. To get an "Application DPK" (the `ps_app_home` tarball), you grab it from the PeopleSoft Image.
~~~ENDSECTION~~~

!SLIDE bullets

# DPK Skillsets

* Basic PS Admin Knowledge
* Hiera, Facter, and YAML
* Puppet
* Scripting Experience
* Ruby

~~~SECTION:notes~~~
The skills listed here are in order of importance. You HAVE to know basic PS Admin to do anything more than build PI's with the DPK. 

You won't need to know Ruby, but if you do you can really make the DPK work for you.
~~~ENDSECTION~~~


!SLIDE bullets

# DPK Considerations

* Built for the PeopleSoft Images
* Windows DPK sometimes behind in features
* Not designed for maintenance*
* Requires `root` access (not in 8.57+)

~~~SECTION:notes~~~
The DPK was built to spin up new environments on new servers, and it's really good at that. That's what "the cloud" needs.

The Windows DPK doesn't get the same love. Some features, like CPU patching via OPatch, have not been delivered for the Windows DPK.

The biggest limitation: the DPK is designed to build and has gaps when using it for maintenance. If you run `puppet apply`, the DPK will shut down every domain it knows about while it runs and then restart the domains. This means you cannot run `puppet apply` if users are on the system. There are ways to mitigate this behavior - we'll cover them - but it's a big issue.

Despite this, the DPK is a fantastic tool and still worth the investment.
~~~ENDSECTION~~~