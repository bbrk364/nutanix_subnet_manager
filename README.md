![](/images/subnet_icon.png)

# Nutanix Subnet Manager
An Ansible role that will create or delete a subnet in a Nutanix cluster running AHV via Prism Central

## Required Variables

**prism_central:** 172.16.1.100  
*FQDN or IP address of the Prism Central server*  
 
**prism_user:** admin  
*An account with permissions to manage subnets*  
 
**prism_password:** super_secret_password  
*The account password should be stored in an Ansible vault file*  
 
**cluster_name:** test-cluster  
*The name of the cluster in which to create or delete a subnet*  
 
**subnet_name:** test-subnet  
*The name of the subnet to be created or deleted*  
> **Note:** *The value is case sensative and must match the exact name as it appears in Prism Central*

**vlan_id:** 999  
*The VLAN ID of the subnet to be created or deleted*
> **Note:** *The value must be an integer*  

**subnet_action:** create  
*Valid options are create or delete depending on the desired action*

## Optional Variables

**global_debug:** True  
*Valid options are True or False whether to show more verbose output (Default value is False)*  

## Sample Playbook
```
---
- name: Create A Subnet In A Nutanix Cluster
  hosts: localhost
  gather_facts: False
  connection: local

  vars:
    prism_central: "prismcentral.domain.com"
    prism_user: "admin"
    prism_password: "super_secret_password"
    cluster_name: "test-cluster"
    subnet_name: "test-subnet"
    vlan_id: 999

  tasks:
    - name: Creating Test Subnet
      include_role:
        name: nutanix_subnet_manager
      vars:
        subnet_action: create

    - name: Pausing To Validate The Test Subnet Was Created
      pause:
        seconds: 30

    - name: Deleting Test Subnet To Cleanup
      include_role:
        name: nutanix_subnet_manager
      vars:
        subnet_action: delete
```
