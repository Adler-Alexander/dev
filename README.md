![alt text](https://github.com/Adler-Alexander/dev/blob/main/media/CCAforSplunk_orange.png)
<img align="right" src="https://badgen.net/badge/Latest%20Premium%20Version/2023.3.1/green?icon=github"><img align="right" src="https://badgen.net/badge/Latest%20Release/2023.2.2/green?icon=github">
### A full lifecycle management interface for Splunk

Ever wished you had a central interface to interact with all aspects of Splunk architecture and administration? 
Let's be honest, running Splunk is all about finding an efficient and scalable way to manage all .conf files and the other magic under the hood. At scale, the complexity often gives way to either speed or quality - if you don't find a way to automate it.

That is precisely what we've done for years, and now it's time to share how you can do it to. Our solution enables a full lifecycle management of Splunk using a **C**ontinuous **C**onfiguration **A**utomation framework powered by Ansible.

>[!NOTE]
> Please note that this is the free open-source version of this automation framework, a trickledown version from our premium option. All development is being made in a seperate repository.


## Table of Contents
- [What is CCA for Splunk](#what-is-cca-for-splunk)
- [Commercial use/support](#commercial-version-of-cca-for-splunk)
- [Features](#features)
- [How to get started](#how-to-get-started)
- [Useful links to deepdive](#Deep-dive)

## What is CCA for Splunk? 
The templates that we provide for configuring Splunk roles are used in our own Multisite Cluster implementations. After you have configured your project, the control is in your hands when it comes to deciding your settings. Adding or modifying parameters has no impact on the framework and are localized under your control.

Playbooks are DRY (Don't Repeat Yourself), with almost no tasks - instead they are using common code in roles. So an update of a task has just to be done in one place, keeping code updates much cleaner and easier to overview.

You can find a more indepth Project Presentation as well as a Q&A section in the [Wiki](https://github.com/innovationfleet/cca_for_splunk/wiki).

![alt text](https://www.orangecyberdefense.com/fileadmin/_processed_/d/8/csm_Splunk_vs_2_45d2f9bce5.png)

For a deep-dive in the technology behind CCA for Splunk please have a look at this documentation.
[Technical documentation](/docs/TechnicalOverview.md).

### Where does CCA for come from and who supports it?

The framework concept utilized in CCA for Splunk goes back several years and has proven to be absolutely critical in managing complex Splunk infrastructures with 100+ servers in several environments. 450+ tasks has been developed across 10 carefully created Ansible roles. We continuously invest **hundreds of development hours for every release**, so that you can get the scalability that you should expect out of a automation framework.
Besides adding your servers to the ansible inventory file, there is less than 25 parameters that you have to set per environment - then off you go to much different Splunk journey going forward.

This is the free open-source version of this automation framework, a trickledown version from our premium option but with all features needed to administrate any size of Splunk environment.



## Commercial version of CCA for Splunk? 
CCA for Splunk is designed to be a companion tool for Splunk administrators in any type of Enterprise. As any tool, it requires a lot of competence from the user to wield effectively. For Splunk Enterprise or Splunk Cloud customers who want to start their automation journey with CCA for Splunk with support and additional enterprise functionality, we offer a complete package of both technology and supporting services in the CCA for Splunk Premium portfolio.

Visit our [CCA for Splunk - Premium](https://www.orangecyberdefense.com/se/cca-for-splunk) page and read more about who backs this project and what else you can do with CCA for Splunk.


## Features
### Open-source
  * Feature 1
  * Feature 2
### Premium
  * Solution Packs
    * Cloud LCM
    * ...
  * Premium Feature 1
  * ....



## How to get started
1: **Plan your architecture**

  - CCA for Splunk can deploy anything from standalone servers to multisite clusters, and up to 9 clusters in each environment, controlled by the same automation framework. A proper planning is key to define the type of architecture(s) that will be created, their environment, individual specifications and requirements.
  <br>

2: **Setup the CCA manager**

  - The CCA manager is the host that orcastrates and manages the automation and configuration deployment.
    There are currently two ways to deploy the manager. 
      1. Use the docker image for cca_for_splunk
      2. Setup the manager on a regular host and pull CCA for Splunk. 

For more in depth information check this guide: [Setup CCA Manager](/docs/SetupCCAManager.md)
  <br>

3: **Setup your environment**<br>
  Watch the video to see the steps of setup manager before you continue.

[![cca_for_splunk Setup Wizard](https://asciinema.org/a/567633.svg)](https://asciinema.org/a/567633)
For more in depth information check this guide: [Setup CCA Manager - Environment](/docs/SetupCCAManager.md#setup-the-environment)



4: **Update ansible inventory and variables**
For more in depth information check this guide: [Setup CCA Manager - Ansible configuration](/docs/SetupCCAManager.md#update-ansible-inventory-files-and-variable-values)
<br>

5: **Validate your environment variables**

Before you start using CCA after an updating to a new release, run the playbook `validate_cca_infrastructure_parameters.yml` to verify that all files in your `cca_splunk_infrastructure` repo are up to date with the required versions in the CCA framework. The verification needs to run in check mode, see command below.

To run an infrastructure playbook:
```
cd ~/data/main/cca_splunk_infrastructure
./cca_ctrl -c
```
<br>

6: **Configure environment using CCA**
If you have servers that is not yet setup for Splunk Enterprise, start by running the `configure_linux_servers.yml` playbook that will prepare the server with users, services and settings to install Splunk Enterprise on it. See [README.md](/roles/cca.core.linux/README.md)
for cca.core.linux role.

When the server configuration is completed, run playbook for managing one of the architectures you want to setup.

If you are to install a multisite index and search head cluster. Start with configuring the index cluster using the playbook [manage_index_clusters.yml](/playbooks/manage_index_clusters.yml) before you run the playbook [manage_searchhead_cluster.yml](/playbooks/manage_searchhead_clusters.yml)
<br>

7: **Onboard data and apps**

Now when your Splunk infrastructure is running smooth, it's time to onboard data and apps. Follow the documentation at [cca.splunk.onboarding](/roles/cca.splunk.onboarding/README.md). When the apps and configuration are completed, run one of the deploy_* playbooks to deploy your apps to the destination server.

To run an onboarding playbook:
```
cd ~/data/main/cca_splunk_onboarding
./cca_ctrl -c
```

>[!NOTE]
> Don't forget that we offer the service to setup and support CCA for you! Please check out our premium feature if this is off interest. [CCA for Splunk - Premium](https://www.orangecyberdefense.com/se/cca-for-splunk)

