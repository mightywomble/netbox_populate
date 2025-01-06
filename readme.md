# Requirements
Netbox in Ansible requires additional modules to be able to run the above scripts, these are supplied by Netbox and installed using ansible-galaxy
Note: This has been tested on Debian and Ubuntu, but should run on rpm-based distros as well, remember you need to install ansible and pip3 on your distro.

- `pip3 install pynetbox`

- `ansible-galaxy collection install netbox.netbox -f`

## Folders

- **netboximport/** - contains the CSV files provided in a YAML format ready to be mergerd with group_vars/all.yaml

- **importfiles/** - contains the raw csv files containing the systems data

- **group_vars/** - contains all.yaml, a dynamically created file (using jenkins) which contains all the data to be imported into Netbox


## Ansible Files

You will see at the top of any playbooks working with netbox the following

`  vars:
    install_state: present
    NETBOX_URL: https://netbox.lan
    NETBOX_TOKEN: dfe756822ff5055b0e9d8bf26fe0b2d10f5c58a0
`
The API token is generated within Netbox and the URL should be a FQDN which Netbox can be reached on.


# Populate Netbox
Before any hosts can be installed, the netbox environment needs to understand about its own setup, IP subnets, racks, vendor information etc. To to this the file 
https://github.com/mightywomble/netbox_populate/blob/main/group_vars/all.yml needs to be updated with the rquired information.

Once this is complete, populate netbox using


`ansible-playbook 00_setup_netbox_prereq.yaml `

The result of this population should be the various manufacturers, sites etc are visible in Netbox

**TODO: create a script for pulling data for all.yaml from a csv file**




# Ansible Ingress YAML  Files
**migratecsv_.yaml** - any ansible code that starts migratecsv_ is used to convert the CSV file to a YAML file which can be used to import the data. 

`ansible-playbook migratecsv_tags.yaml`

`ansible-playbook migratecsv_hosts.yaml`

These playbooks will 
- take the content in importfiles/
- use the template/*.j2 templates to migrate csv into yaml
- output the yaml files in netboximport/


*Note: This should really be a loop in a single YAML file which runs through the CSV's expected in the folder.*



# Ansible Ingress YAML  Netbox Import
To import the hosts and tags into netbox run

`ansible-playbook 02_add_devices.yaml`

# READING
1. https://github.com/jvanderaa/ansible_netbox_demo
2. https://blog.networktocode.com/post/netbox_as_ansible_sot/
3. https://ttl255.com/developing-netbox-plugin-part-1-setup-and-initial-build/
