## Dashboard Branding Notes and Instructions

These are notes and instructions for developing the Open OnDemand (OOD) dashboard with NCAR branding. There are two "classes" of files involved in the NCAR branding: static assets and dynamic Ruby templates. While the Ruby templates reference the static assets, the two classes of files are handled and configured very differently. 

Static assets (css, images) are accessible from the Apache public assets folder: /var/www/ood/public by default. While the public subdirectory is configurable, the Apache root /var/www/ood is not. Static assets are served relative to the Apache root regardless of whether the dashboard is the system dashboard or a development dashboard within a developer's "sandbox". The Apache root is fixed at install. The development dashboard may use different static assets but they still are expressed relative to the Apache root.

Customized views and layouts in the form of dynamic Ruby templates, like our NCAR header and footer, live under the top level OOD configuration directory /etc/ood/config, specifically here: /etc/ood/config/apps/dashboard. A development dashboard may use a different application configuration directory via environment variable configuration to identify where these customizations reside.

We can, through configuration settings and environment variables, fully "insulate" dashboard development from the system dashboard. By setting the variable public_url (which is really a directory postfix to the Apache root /var/www/ood) to a subdirectory belonging to an individual developer, we can reference the developer's static assets and not the production static assets. This variable is used automatically by OOD to locate custom css files. We can use it when coding .erb files as well when we need to specify images (NCAR logos). For example within an .erb file specify: src="<%= @user_configuration.public_url%>/logo-ncar.png": Note, however, such references using public_url remain relative to the Apache root.

To develop customized views and layouts (the dynamic Ruby templates or .erb files), we can use the environmental variables OOD_LOAD_EXTERNAL_CONFIG and OOD_APP_CONFIG_ROOT to fully relocate the directory from which these files are read. Lastly, environmental variable OOD_CONFIG_D_DIRECTORY allows us to relocate the directory where top level yaml configuration files reside. Crucially, variable public_url is set in these files--typically however there is just one file called ondemand.yml 

HSG Admins assist us setting up OOD for dashboard development (really for the development of any "sandbox" interactive application we want to play with). They install OOD with Ansible--a tool that provisions virtual machines based on a repository describing the installation's configuration. One of the tasks Ansible performs when installing the prototype instance of OOD is enabling "development mode" for specific users with unix accounts on the prototype machine. Development mode allows such users to deploy interactive applications beyond what is available with the system installation of OOD for the purposes of developing and testing them. These development versions of interactive applications are available in what is called the "sandbox". See the "My Sandbox Apps" menu entry to access them. For our purposes here, the dashboard itself is one of these interactive applications.

See https://osc.github.io/ood-documentation/latest/how-tos/app-development/enabling-development-mode.html

A second Ansible task prepares developers so that they may update and test new NCAR branding onboard their development dashboards; specifically, it creates the filesystem structure (read, directories and symlinks) that match the value that the variable public_url will have for the developer.

## Steps to Create the Development Dashboard

Additional steps below must be taken by the developer to create the development or "sandbox" dashboard. Follow the steps in the example below to complete the creation of a development dashboard. The example uses username jcunning--change to your username where found.

Post the execution of the two Ansible tasks mentioned above, a developer should see the following on the Prototype OOD VM:

  - The symlink /var/www/ood/apps/dev/jcunning/gateway pointing to ~jcunning/ondemand/dev
  - The symlink /var/www/ood/public/dev/jcunning pointing to ~jcunning/ondemand/public

(Note: /var/www/ood is the Apache root)

Once a developer is setup as described above, the developer needs to execute the following steps to complete the development dashboard with its NCAR branding:

  - mkdir -p ~/ondemand/src ~/ondemand/dev ~/ondemand/ondemand.d
  - cd ~/ondemand/src
    - git clone https://github.com/NCAR/sage-ood-dashboard-branding.git
    - git clone https://github.com/OSC/ondemand.git
  - cd ~/ondemand/public
    - ln -s ../src/sage-ood-dashboard-branding/public/branding branding
  - cd ~/ondemand/dev
    - ln -s ../src/ondemand/apps/dashboard dashboard
    - cd dashboard
      - git checkout v4.0.8 # checkout the tag matching the installation of OOD on the VM!
      - ./bin/setup # builds the dashboard app
      - Create file .env.local with the following content:
```
OOD_LOAD_EXTERNAL_CONFIG=1
OOD_CONFIG_D_DIRECTORY="/glade/u/home/jcunning/ondemand/dev/dashboard/config/ondemand.d" # OOD bug with tilde processing present on this setting!!
OOD_APP_CONFIG_ROOT="/glade/u/home/jcunning/ondemand/src/sage-ood-dashboard-branding/apps/dashboard" # OOD bug with tilde processing absent here
```
      - Create file .env.overload with the following content:
```
OOD_DASHBOARD_TITLE="NSF NCAR HPC OnDemand"
OOD_DASHBOARD_LOGO="/public/dev/jcunning/branding/NSF-NCAR_Logo_FullColor_RGB.png"
OOD_DASHBOARD_LOGO_HEIGHT="200"
OOD_BRAND_BG_COLOR: "#0057C2"
OOD_BRAND_LINK_ACTIVE_BG_COLOR: "#00357A"
```
  - cd ~/ondemand/ondemand.d
    - cp /etc/ood/config/ondemand.d/ondemand.yml . # note this is the installation's ondemand.yml being copied
    - Edit the copy of ondemand.yml and add or update public_url to the following:
```
public_url: "/public/dev/jcunning" # note the lack of a trailing slash here--this has implications when using public_url in .erb files
```

Important, the environment variables in the file .env.overload above override the equivalent settings in the file /etc/ood/config/nginx_stage.yml, which is a high level configuration file for configuring the Per User Nginx server. These environment variables are usable by any interactive application not just the dashboard. And, except for the OOD_DASHBOARD_LOGO, changes to the variables used above have to be communicated specifically by value to the HSG admins as they are not captured in the sage-ood-dashboard-branding repository.

Note, the above is an ever so slightly modified set of instructions for setting up a development dashboard from the original OOD instructions. See https://osc.github.io/ood-documentation/latest/tutorials/tutorials-dashboard-apps.html

The development dashboard should now be available in the "My Sandbox Apps" with the NCAR branding onboard but using the developer's static assets and dynamic views and layouts within the clone of the sage-ood-dashboard-branding repository.

## Tagging for Release

The sage-ood-dashboard-branding repository is consumed by Ansible when OOD is installed both on the prototype and production VMs. The Ansible task clones the sage-ood-dashboard-branding repository on the target VM and checks out the appropriate commit by a configured tag. The tag format is prefixed by the corresponding production release tag of the ondemand installation and is completed by a version stamp of the branding (ex. v4.0.8_1.0.0). When ready to release updated branding, tag sage-ood-dashboard-branding accordingly with a new tag in this format.
