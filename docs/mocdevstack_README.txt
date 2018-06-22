MOC Devstack


Launch Instructions:
- Launch a VM with $RAM, $CPU, $NETWORKING, $OS, $FLAVOR, $ETC
   - In the "Configuration" section, launch with the single_node_devstack.yml
   cloud-config script
- on the machine, switch to stack user: `sudo su - stack`
- enter the devstack directory: `cd devstack`
- Edit local.conf as desired
- install + run DevStack: `./stack.sh`
- Installation may take up to 20 minutes

--------------------------------------------------------------

Local.conf:

/opt/stack/devstack/local.conf
- set the openstack passwords
- specify the repo/branch/commit for services
- enable or disable plugins like rally, osprofiler, etc

---------------------------------------------------------------

Repos, Branches, and Commits (oh my!):

To use a specific repo, branch, or commit, before running stack.sh
edit local.conf to add:

$SERVICE_REPO=<git repo url>
$SERVICE_BRANCH=<branch name>
$SERVICE_COMMIT=<commit sha>

Turn reclone on to make sure that devstack fetches the specified
repository and uses it for the openstack service.

--------------------------------------------------------------

Openstack Services:

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


