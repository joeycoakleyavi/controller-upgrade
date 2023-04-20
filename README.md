# Ansible Playbook for Automating Controller Upgrades

## Summary
This playbook and collection of roles allow an administrator to specify either a controller base image or patch to upgrade. It will determine whether the image requires an upload to the controller, and will upload if necessary. From there, it will determine if the image is a patch or system image, then perform the appropriate operation.

Please define variables in the `vars.yml` file before use. The ansible.cfg currently uses ./hosts.yml for the inventory. Either edit this file with your controller information, or use your own inventory file. Multiple clusters are supported so long as they are performing the same upgrade and are on the same API version.

## Getting Started
```
git clone git@github.com:joeycoakleyavi/controller-upgrade.git
cd controller-upgrade
python3 -m venv env
source env/bin/activate
pip3 install -r requirements.txt
ansible-galaxy collection install vmware.alb

Edit hosts.ini to include which controllers should be upgraded. The playbook currently points to **ALL** hosts in this file. Scope this down if required.

Note: Only include the leader node of a given cluster in hosts.yml. Avi Controllers will handle distributing the image and performing upgrades to all member controllers of the cluster.

For Non-GSLB Avi Clusters:
ansible-playbook upgrade_non_gslb.yml

For GSLB-Enabled Clusters:
ansible-playbook upgrade_gslb.yml

##optional variables
disruptive_upgrade: default is false
suspend_on_failure: default is true
action_on_error: default is SUSPEND_UPGRADE_OPS_ON_ERROR (This is for SE upgrades, possible options: ROLLBACK_UPGRADE_OPS_ON_ERROR, SUSPEND_UPGRADE_OPS_ON_ERROR, CONTINUE_UPGRADE_OPS_ON_ERROR) 

```