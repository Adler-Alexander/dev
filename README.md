![alt text](https://github.com/Adler-Alexander/dev/blob/main/docs/img/CCAforSplunk_orange.png)
<img align="right" src="https://badgen.net/badge/Latest%20Premium%20Version/2023.3.1/green?icon=github"><img align="right" src="https://badgen.net/badge/Latest%20Version/2023.2.2/green?icon=github">
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
- [Technical overview](#technical-overview)
- [How to get started](#how-to-get-started)
- [Useful links to deepdive](#Deep-dive)

## What is CCA for Splunk? 
The templates that we provide for configuring Splunk roles are used in our own Multisite Cluster implementations. After you have configured your project, the control is in your hands when it comes to deciding your settings. Adding or modifying parameters has no impact on the framework and are localized under your control.

Playbooks are DRY (Don't Repeat Yourself), with almost no tasks - instead they are using common code in roles. So an update of a task has just to be done in one place, keeping code updates much cleaner and easier to overview.

You can find a more indepth Project Presentation as well as a Q&A section in the [Wiki](https://github.com/innovationfleet/cca_for_splunk/wiki).
![alt text](https://www.orangecyberdefense.com/fileadmin/_processed_/d/8/csm_Splunk_vs_2_45d2f9bce5.png)

### Where does CCA for come from and who supports it?

The framework concept utilized in CCA for Splunk goes back several years and has proven to be absolutely critical in managing complex Splunk infrastructures with 100+ servers in several environments. 450+ tasks has been developed across 10 carefully created Ansible roles. We continuously invest **hundreds of development hours for every release**, so that you can get the scalability that you should expect out of a automation framework.
Besides adding your servers to the ansible inventory file, there is less than 25 parameters that you have to set per environment - then off you go to much different Splunk journey going forward.

This is the free open-source version of this automation framework, a trickledown version from our premium option but with all features needed to administrate any size of Splunk environment.



## Commercial version of CCA for Splunk? 
CCA for Splunk is designed to be a companion tool for Splunk administrators in any type of Enterprise. As any tool, it requires a lot of competence from the user to wield effectively. For Splunk Enterprise or Splunk Cloud customers who want to start their automation journey with CCA for Splunk with support and additional enterprise functionality, we offer a complete package of both technology and supporting services in the CCA for Splunk Premium portfolio.

Visit our [CCA for Splunk - Premium](https://www.orangecyberdefense.com/se/cca-for-splunk) page and read more about who backs this project and what else you can do with CCA for Splunk.


## Features
### Open-source
### Premium


## How to get started
**Step 1: Plan your architecture**

CCA for Splunk can deploy anything from standalone servers to multisite clusters, and up to 9 clusters in each environment, controlled by the same automation framework.
A proper planning is key to define the type of architecture(s) that will be created, their environment, individual specifications and requirements.
<br>
**Step 2: Setup the CCA manager**

The CCA manager is the host that orcastrates and manages the automation and configuration deployment.
There are currently two ways to deploy the manager. 
1. Use the docker image for cca_for_splunk
2. Setup the manager on a regular host and pull CCA for Splunk. 

For more in depth information check this guide: [Setup CCA Manager](/docs/SetupCCAManager.md)
<br>

**Step 3: Setup your environment**<br>

Watch the video to see the steps of setup manager before you continue.

[![cca_for_splunk Setup Wizard](https://asciinema.org/a/567633.svg)](https://asciinema.org/a/567633)


**b**) run `./cca_ctrl --setup` from the **cca_for_splunk** repo. The wizard will ask you to provide the following information:

* Name of your environment (cca_lab)

When this information is collected, template files will be copied from the **cca_for_splunk/templates** directory to build the base for the two required repositories, when this is completed you will be asked to provide the following information:

* Splunk Secret, this is the key used to encrypt and decrypt Splunk Passwords and secrets. Accept the generated key or provide your own.
* The name of the admin user (admin)
* The password for the admin user, a random password is generated. Store it if you choose to use it.
* The general pass4SymmKey that is used by Splunk for S2S communication, like communication to license managers. If you have an existing infrastructure, use that pass4SymmKey. If not keep the random key.

Next comes a generation of 4 different sslpasswords, server, web, inputs and outputs. CCA for Splunk can deploy 4 unique certificates to the infrastructure. If you already have certificates that can be used, use the password that correlates to the respective private key. Read more about [certificates](/roles/cca.splunk.ssl-certificates/README.md). Otherwise use the given passwords when generating the encrypted private keys.

* Password for Server Certificate
* Password for Inputs Certificate
* Password for Outputs Certificate
* Password for Web Certificate

The wizard assumes that you have 1 index and 1 search head cluster and will prompt for pass4SymmKeys for those clusters. If you have an existing cluster, add the existing pass4SymmKey instead of the pre-generated one.

* Cluster C1 pass4SymmKey
* Search Head Cluster C1 pass4SymmKey

In the background 8 more pass4SymmKeys for respective cluster is generated and stored in the Splunk Infrastructure repo at environments/ENVIRONMENT_NAME/group_vars/all/cca_splunk_secrets

To access the cleartext value of one of the ansible secrets, run the following command from the relevant repo. Replace the variable specified for `var`.
```
ansible -i environments/cca_lab -m debug -a "var=cca_splunk_certs_server_default_sslpassword" localhost
```

**c**) Verification: Verify that two companion repos has been created and staged with the correct information.

**Step 4:**  Update ansible inventory files and variable values in the following files in your environment directory.

* group_vars/all/env_specific
  * cca_splunk_license_manager_uri: 'https://UPDATE_LICENSE_MANAGER_FQDN:8089'
  * domain_name: 'UPDATE_DOMAIN.NAME'
  * cca_splunk_alert_action_smtp: 'UPDATE_SMTP_SERVER_FQDN'
  * cca_splunk_health_alert_action_email_to: 'UPDATE_ALERT_EMAIL_ADDRESS'
  * cca_splunk_extension_cert_rootca: 'UPDATE_ROOT_CA_EXPIRE_DATE_Issuing_CA_EXPIRE_DATE.pem'
  * cca_splunk_extension_server_cert: 'splunk-server_UPDATE_EXPIRE_DATE.cer'
  * cca_splunk_extension_server_key: 'splunk-server_UPDATE_EXPIRE_DATE.key'
  * cca_splunk_extension_inputs_cert: 'splunk-inputs_UPDATE_EXPIRE_DATE.cer'
  * cca_splunk_extension_inputs_key: 'splunk-inputs_UPDATE_EXPIRE_DATE.key'
  * cca_splunk_extension_outputs_cert: 'splunk-outputs_UPDATE_EXPIRE_DATE.cer'
  * cca_splunk_extension_outputs_key: 'splunk-outputs_UPDATE_EXPIRE_DATE.key'
  * cca_splunk_extension_web_cert: 'splunk-web_UPDATE_EXPIRE_DATE.cer'
  * cca_splunk_extension_web_key: 'splunk-web_UPDATE_EXPIRE_DATE.key'
  * 'UPDATE_LICENSE_FILE.lic'
* group_vars/all/linux
  * splunk_user_uid: 'UPDATE_SPLUNK_UID'
  * splunk_user_gid: 'UPDATE_SPLUNK_GID'
  * firewall_zone_name: 'UPDATE_ZONE_NAME'
  * firewall_zone_description: 'UPDATE_ZONE_DESCRIPTION'
* hosts
  * UPDATE Splunk S2S ports if the default don't match your environment.
  * UPDATE Splunk enterprise version to your desired version.
  * ansible_ssh_user="UPDATE_SSH_REMOTE_USER"
  * UPDATE search and replication factor to match your environment
  * UPDATE available sites to match your environment
  * UPDATE hot and cold volume path to match your environment
  * maxVolumeDataSizeMB_hot="UPDATE_HOT_VOLUME_SIZE_MB"
  * maxVolumeDataSizeMB_cold="UPDATE_COLD_VOLUME_SIZE_MB"

**Step 5:**

Before you start using CCA after an updating to a new release, run the playbook `validate_cca_infrastructure_parameters.yml` to verify that all files in your `cca_splunk_infrastructure` repo are up to date with the required versions in the CCA framework. The verification needs to run in check mode, see command below.

```
cd ~/data/main/cca_splunk_infrastructure
./cca_ctrl -c
```

To run an infrastructure playbook:
```
cd ~/data/main/cca_splunk_infrastructure
./cca_ctrl -c
```



If you have servers that is not yet setup for Splunk Enterprise, start by running the `configure_linux_servers.yml` playbook that will prepare the server with users, services and settings to install Splunk Enterprise on it. See [README.md](/roles/cca.core.linux/README.md)
for cca.core.linux role.

When the server configuration is completed, run playbook for managing one of the architectures you want to setup.

If you are to install a multisite index and search head cluster. Start with configuring the index cluster using the playbook [manage_index_clusters.yml](/playbooks/manage_index_clusters.yml) before you run the playbook [manage_searchhead_cluster.yml](/playbooks/manage_searchhead_clusters.yml)

**Step 6:**

Now when your Splunk infrastructure is running smooth, it's time to onboard data and apps. Follow the documentation at [cca.splunk.onboarding](/roles/cca.splunk.onboarding/README.md). When the apps and configuration are completed, run one of the deploy_* playbooks to deploy your apps to the destination server.

To run an onboarding playbook:
```
cd ~/data/main/cca_splunk_onboarding
./cca_ctrl -c
```
