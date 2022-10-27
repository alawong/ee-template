# aa-ee-template

A template repository for a new EE.

This repository also includes a template ansible.cfg file that will allow you to download and install collections stored within Red Hat Automation Hub, Ansible Galaxy, and the AAP Private Automation Hub.

For Red Hat Automation Hub and Private Automation Hub, their offline authentication tokens will need to be injected into the `ansible.cfg` template file.

An example EE configuration has been provided within the `example/` directory. This directory should be deleted prior to committing and pushing the code to the new EE's remote repository.

It is recommended that the base and builder images (used to build the EE upon) remain the same. These images contain the minimum requirements to run Ansible core.

## Using aa-ee-template

### Requirements

The following is required prior to building an EE:

* Access to the Red Hat Container Registry at ([registry.redhat.io]
* The following CLI tools
  * ansible-builder to create the Containerfile used to build the EE
  * podman to login to the Red Hat Container Registry and Tennet Container Registry (TCR), and to build the EE
* Ansible playbook dependencies
  * System packages (e.g. via an RPM)
  * Python packages (i.e. via pip)
  * Ansible collections (these can be sourced from Red Hat Automation Hub, Ansible Galaxy, or can be custom collections downloaded from the AAP2 Private Automation Hub)
    * For Red Hat Automation Hub and Private Automation Hub collections, their respective API offline tokens are required

Initialise your new EE repository within Bitbucket, following the naming convention `aa-ee-<< EE name >>`. Clone both the new repository, and `aa-ee-template` onto your server.

Change directory into the repository. Then, copy the contents of this repository into your new repository `cp aa-ee-template/ .`.

Remove any dependency files that are unrequired, and add the dependencies in.

## Building the Execution Environment

Login to the Red Hat Container registry by running the command `podman login registry.redhat.io`, and entering the username and password when prompted.

Run `ansible-builder create` within the root directory of the EE repository to create the `context/` directory. This will also create the Containerfile which is used to build the EE.

Run `podman build -t << TCR Hostname >>/<< EE name >>:<< version >> -f context/Containerfile` to build the EE. Once the build has completed, the EE can be used Automation Controller once pushed to TCR, and also via the CLI tool `ansible-navigator`

Push the EE to TCR by logging into TCR via the TCR hostname `podman login << TCR hostname >>`, and then pushing it using podman `podman push -t << TCR Hostname >>/<< EE name >>:<< version >>`.
