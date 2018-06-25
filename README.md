# more
### MOC OpenStack Research Environment

The MOC Openstack Research Environment is a two-pronged suite of testing tools for MOCers. It consists of a development environment (a single-node modified [DevStack](https://docs.openstack.org/devstack/latest/) environment for quickly testing changes and running smaller experiments) and a production-like environment (a multi-node modified [TripleO](https://docs.openstack.org/tripleo-docs/latest/) environment for testing changes and running experiments at production-level scale).

--------------------------------------------------------------

## Development Environment

DevStack is a collection of scripts designed to quickly deploy an OpenStack environment on a single node. MOC DevStack is a patch designed to be compatible with CentOS and add some functionality.

#### Launch Instructions:
- Launch a CentOS VM with $RAM, $CPU, $NETWORKING, $FLAVOR, $ETC
   - In the "Configuration" section, launch with the single_node_devstack.yml
   cloud-config script
- On the machine, switch to stack user: `sudo su - stack`
- Enter the devstack directory: `cd devstack`
- Edit local.conf as desired
- Install + run DevStack: `./stack.sh`
- Installation may take up to 25 minutes (enabled plugins may increase install time)

--------------------------------------------------------------

#### Local.conf:

/opt/stack/devstack/local.conf
- set the openstack passwords
- specify the repo/branch/commit for services
- enable or disable plugins like rally, osprofiler, etc

--------------------------------------------------------------

#### Repos, Branches, and Commits (oh my!):

To use a specific repo, branch, or commit, before running stack.sh
edit local.conf to add:

$SERVICE_REPO=<git repo url>
$SERVICE_BRANCH=<branch name>
$SERVICE_COMMIT=<commit sha>

After running stack.sh, the repo/branch/commit can be modified
for an individual service using git commands in the service
directory at /opt/stack/$SERVICE as indicated below.

--------------------------------------------------------------

#### Openstack Services:

To test large changes to Openstack source code, first ensure 
that the proper repos are specified in your local.conf. Commit
your changes to the git tree, then enter the /opt/stack/$SERVICE
folder for the service modified. Run `git pull` to get your latest
changes. Then, in the /opt/stack/devstack folder run `./unstack.sh` 
and `./stack.sh` to restart the devstack service. This will take 
15-20 minutes for devstack to rebuild.


To test smaller changes, you can edit the code on the machine
itself. Source code for openstack services is located in
`/opt/stack/$SERVICE`. After making a change, restart openstack
services by running:
`sudo systemctl restart "devstack@*"`

*note that these changes are in checked out git trees, so if you
do not commit changes using the second method, your work may
be overwritten by subsequent DevStack runs*

--------------------------------------------------------------



## Production-Like Environment

WIP
