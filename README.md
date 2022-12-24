# ee-template

A template repository that allows for automation of EE builds.

It is recommended that the base and builder images (used to build the EE upon) remain the same. These images contain the minimum requirements to run Ansible core.

## Using ee-template

### Requirements

The following is required prior to building an EE:


* Access to the Red Hat Container Registry at link:registry.redhat.io
* An Ansible host with the following:
  * CLI tools:
    * ansible-core or ansible
    * ansible-builder
    * podman
  * redhat_cop.ee_utilities collection installed (and therefore containers.podman)
* Dependencies required to run target Ansible playbooks:
  * System packages
  * Python packages (i.e. via pip)
  * Ansible collections (these can be sourced from Ansible Automation Hub, Ansible Galaxy, or from your Private Automation Hub)
    * For Ansible Automation Hub and Private Automation Hub collections, their respective hostnames and API offline tokens are required

Initialise your new EE repository, following the naming convention `ee-<< EE name >>`. Clone both the new repository, and _ee-template_ onto your Ansible host.

Copy the contents of the _ee-template_ repository into the new EE repository.

`cp -r ee-template/ ee-xxx`

Add the EE requirements into ee_build.yml under the `ee_list` variable. Remove any requirements (python, galaxy, or bindep) that are unneeded.

Add the following credentials to vars/variables.yml:

* registry.redhat.io details - used to pull the base and builder images
* Private Automation Hub details - used to install Ansible collections
* Container Registry - where the EE will be pushed to once built

## Building the Execution Environment

Run the command `ansible-playbook ee_build.yml --vault-password-file /path/to/file`

Verify that the EE has been built by running the command `podman pull << registry_hostname >>/<< EE name >>:<< version >>`.
