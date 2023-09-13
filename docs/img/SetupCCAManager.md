## Setup the CCA Manager

To use the docker image for CCA for Splunk follow the instruction in
[CCA for Splunk - Docker Repo](https://github.com/innovationfleet/docker).
When completed you can jump to setup the environment.

**Step 2: Alt 2: Install the Manager and pull CCA for Splunk**
Machine minimum requirements:
 CPU: 2core
 RAM: 4GB
 Disk: 40GB
 preferred OS: RHEL 8 or higher, CentOS 8 stream or higher

**a)** from the cca_for_splunk repo, run the Readiness playbooks to ensure that you have the prerequisites and install missing tools/packages:
Follow the instructions in [Setup CCA Manager](#prerequisites)
Read up on our [Automation Readiness](/automation_readiness.md) page.

These playbooks will check your automation readiness of both the Manager server, and your Splunk infrastructure in simple assert tasks. When you have passed all assert checks, your environment is ready for the automation journey to start.