# Requirements
- `pip3 install pynetbox`

- `ansible-galaxy collection install netbox.netbox -f`



# Notes

## Populate Netbox
00_setup_netbox_prereq.yaml 

Before Netbox can be used it needs to be populated with information about racks, ip subnets, device types and sites
Running 

`ansible-playbook 00_setup_netbox_prereq.yaml `

Will run this playbook, the details are fed in from the file group_vars/all.yaml

**TODO: create a script for pulling data for all.yaml from a csv file**

## Folders

- **netboximport/** - contains the CSV files provided in a YAML format ready to be mergerd with group_vars/all.yaml

- **importfiles/** - contains the raw csv files containing the systems data

- **group_vars/** - contains all.yaml, a dynamically created file (using jenkins) which contains all the data to be imported into Netbox


## Ansible Files
**migratecsv_.yaml** - any ansible code which starts migratecsv_ is used to convert the CSV file to a YAML file which can be used to import the data. 

*Note: This should really be a loop in a single YAML file which runs through the CSV's expected in the folder.*


## READING
1. https://github.com/jvanderaa/ansible_netbox_demo
2. https://blog.networktocode.com/post/netbox_as_ansible_sot/
3. https://ttl255.com/developing-netbox-plugin-part-1-setup-and-initial-build/
