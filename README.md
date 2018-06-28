# Openstack Research Environment

The MOC Openstack Research Environment is a two-pronged suite of testing tools for MOCers. It consists of a development environment (a single-node modified [DevStack](https://docs.openstack.org/devstack/latest/) environment for quickly* testing changes and running smaller experiments) and a production-like environment (a multi-node modified [TripleO](https://docs.openstack.org/tripleo-docs/latest/) environment for testing changes and running experiments at production-level scale).

--------------------------------------------------------------

## Development Environment

DevStack is a collection of scripts designed to quickly deploy an OpenStack environment on a single node for testing purposes. MOC DevStack is a patch designed to be compatible with CentOS and add some functionality.

#### Launch Instructions:
- In Kaizen, launch a VM with the CentOS 7.5 image
   - In the "Configuration" section, launch with the [single_node_devstack.yml](../master/single_node_devstack/single_node_devstack.yml) cloud-config script
   
   *Note that VM launch may take up to 5 minutes. You can monitor the progress in the log in Kaizen.*
   
   *VM status will be active, but it will be patching in the background. You can check the instance log on Kaizen to see when this is complete*
- After VM launch complete, SSH to the machine as stack user `ssh stack@<devstack machine floating IP>`
- Enter the devstack directory: `cd devstack`
- Edit local.conf as desired
- Install + run DevStack: `./stack.sh`
- *Installation may take up to 25 minutes (enabled plugins may increase install time)

*If you fork this repo, make sure to edit single_node_devstack.yml to point to your fork. Otherwise, it will continue to download the patch from CCI-MOC*

--------------------------------------------------------------

#### Local.conf:
[local.conf](../master/patch/local.conf)

located on DevStack machine at: /opt/stack/devstack/local.conf

Usage:
- set the openstack passwords (default is "Devstack1" for admin user)
- specify the repo/branch/commit for services
- enable or disable plugins like rally, osprofiler, etc

--------------------------------------------------------------

#### Repos, Branches, and Commits (oh my!):

To use a specific repo, branch, and/or commit, before running stack.sh
edit local.conf to add:

$SERVICE_REPO=<git repo url>

$SERVICE_BRANCH=<branch name>

$SERVICE_COMMIT=<commit sha>

After running stack.sh, the repo/branch/commit can be modified
for an individual service using git commands in the service
directory at /opt/stack/$SERVICE as indicated below.

--------------------------------------------------------------

#### Openstack Services:

Workflow for testing changes to Openstack source code:
1. Specify your custom git repo in local.conf as indicated above
2. Install DevStack `./stack.sh`
3. Make changes to your openstack source code in the repo
4. `git pull` changes into the directory on DevStack machine
5. Restart DevStack: `./unstack.sh` and `./stack.sh`


Another option is to edit the code on the machine itself. This may be advantageous for testing smaller changes if you do not want to go through the git workflow. Source code for openstack services is located in `/opt/stack/$SERVICE`. After making a change, restart all openstack services by running:
`sudo systemctl restart "devstack@*"`

You can also restart individual services after making a change with similar syntax. More info: https://docs.openstack.org/devstack/latest/development.html

*note that these changes are in checked out git trees, so if you
do not commit changes using the second method, your work may
be overwritten by subsequent DevStack runs*

Further reading on openstack services in devstack: https://docs.openstack.org/devstack/latest/systemd.html

--------------------------------------------------------------

#### Modifications to DevStack:

- after cloning requirements repo, change python-libvirt to 3.9 in upper-constraints.txt (this is because CentOS 7 ships with python-libvirt 3.9 but devstack pike sets upper limit of python-libvirt 3.5)
- implement fix in devstack/inc/python to not uninstall when offline mode is enabled
- enable offline mode after a successful install of devstack in order to avoid downloading new (and incompatible) versions of software
- change /inc/$SERVICE to use git reset to allow choosing a specific commit

--------------------------------------------------------------



## Production-Like Environment

WIP
