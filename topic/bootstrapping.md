!SLIDE center subsection blue

# Bootstrapping

!SLIDE bullets

# Bootstrapping

"The process of loading the basic software into the memory of a computer after power-on or general reset, especially the operating system which will then take care of loading other software as needed."

!SLIDE bullets

# What is Bootstrapping?

1. Extract downloaded .zip files
1. Install Puppet
1. Get configuration information
1. Deploys DPK archives
1. Configure server DPK Role
1. Deploy middleware software
1. Deploy PeopleTools/application homes
1. Create web/app/batch domains
1. Run pre-boot ACM
1. Start domains
1. Run post-boot ACM

~~~SECTION:notes~~~
Bootstrapping can be done on a fresh server, or on an existing server. If you want to upgrade PeopleTools, you might run the bootstrap scripts from the newer PeopleTools DPK.
~~~ENDSECTION~~~

!SLIDE bullets

# Bootstrap Options

`setup\psft-dpk-setup.bat(.sh)` is the bootstrap script

1. `--env_type`
  1. `dbtier`
  1. `midtier`
  1. Default value is `fulltier` - a PeopleSoft Image
1. `--domain_type`
  1. `appserver`
  1. `prcs`
  1. `pia`
  1. `appbatch`

~~~SECTION:notes~~~
One role is missing: `piaapp`. We will show how to create that role later on. These options will define the DPK Role for the server. More on that in a bit.
~~~ENDSECTION~~~

!SLIDE bullets

# Bootstrap Options

1. `--deploy_only`
1. `--deploy_type`
  1. `--app_home`
  1. `--tools_home`
  1. `--app_and_tools_home`
1. `--dpk_src_dir`
1. `--no_puppet_run`

~~~SECTION:notes~~~
These options will determine how the bootstrap script functions. Do you want to deploy the new `PS_HOME` files, or `PS_APP_HOME` files, or just deploy the DPK archives but not run Puppet? These options don't affect the DPK Role.
~~~ENDSECTION~~~

!SLIDE bullets

# Bootstrap Details

`--env_type midtier`

1. Application DPK (PI) gives you `pt_app_*` DPK Roles
1. PeopleTools DPK gives you `pt_tools_*` DPK Roles

PeopleSoft Image builds use `fulltier` and result in `pt_<app>_pum` DPK Roles

!SLIDE bullets

# Bootstrap Process

The `psft-dpk-setup` script will:

1. Installing Puppet, Hiera eYAML and copy the DPK modules
1. Extracting any downloaded .zip files
1. Extracting DPK archives into `c:\psft\dpk\archives`
1. Ask you questions, populate YAML files with the answers, and set the DPK Role
