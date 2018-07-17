!SLIDE center subsection blue

# Bootstrapping

!SLIDE bullets

# Bootstrapping

"The process of loading the basic software into the memory of a computer after power-on or general reset, especially the operating system which will then take care of loading other software as needed."

!SLIDE bullets

# What is Bootstrapping?

The DPK bootstrap process:

1. Extract downloaded .zip files
1. Install Puppet
1. Get configuration information
1. Deploys DPK archives
1. Configure server DPK Role

At this point, Puppet will configure the server.

~~~SECTION:notes~~~
Bootstrapping can be done on a fresh server, or on an existing server. If you want to upgrade PeopleTools, you might run the bootstrap scripts from the newer PeopleTools DPK.
~~~ENDSECTION~~~

!SLIDE bullets

# Bootstrap Options

`setup\psft-dpk-setup.bat(.sh)` is the bootstrap script

    @@@console
    --env_type dbtier
    --env_type midtier
    --env_type midtier --domain_type appserver
    --env_type midtier --domain_type prcs
    --env_type midtier --domain_type pia
    --env_type midtier --domain_type appbatch


~~~SECTION:notes~~~
One role is missing: `piaapp`. We will show how to create that role later on. These options will define the DPK Role for the server. More on that in a bit.
~~~ENDSECTION~~~

!SLIDE bullets

# Bootstrap Options

`setup\psft-dpk-setup.bat(.sh)` is the bootstrap script

    @@@console
    --env_type midtier --deploy_only --deploy_type tools_home
    --env_type midtier --deploy_only --deploy_type app_home
    --env_type midtier --deploy_only --deploy_type app_and_tools_home
    --dpk_src_dir
    --no_puppet_run
    --patches_dir path
    --silent

~~~SECTION:notes~~~
These options will determine how the bootstrap script functions. Do you want to deploy the new `PS_HOME` files, or `PS_APP_HOME` files, or just deploy the DPK archives but not run Puppet? These options don't affect the DPK Role.
~~~ENDSECTION~~~