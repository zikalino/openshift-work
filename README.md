# Openshift work

This is temporary repo to do experiments with openshift

## Requirements

Make sure you have ansible 2.6.1, and make sure you have azure cli 2.41. Also, make sure you update azure for pip, especially if you run into not found element errors.

## TL;DR

Install the requirements:

```bash
sudo pip install ansible[azure]
sudo pip install msrestazure
```

Run the playbook:

```bash
ansible-playbook playbooks/create.yml
```

# Proposed structure of deployment

Here is the proposed structure on how we should run all the tasks to deploy Openshift

## Infrastructure

1. Deploy Resource Group
2. Deploy Vnet
3. Deploy simultaneosly basic network items:
    1. Master:
        - Subnet
        - Public IP
        - Network Security Group
        - Availability Set
    2. Infra:
        - Subnet
        - Public IP
        - Network Security Group
        - Availability Set
    3. Nodes:
        - Subnet
        - Network Security Group
        - Availability Set
4. Deploy Load Balancers:
    1. Master LB - Depends on Master Public IP
    2. Infra LB - Depends on Infra Public IP
5. Deploy NICs:
    1. Bastion NIC - Depends on Subnet
    2. Master NICs - Depends on Master Subnet, LB and NSG
    3. Infra NICs - Depends on Infra Subnet, LB and NSG
    4. Node NICs - Depends on Node Subnet and NSG
6. Deploy Virtual Machines :
    1. Bastion
        - VM - Depends on Bastion NIC
        - Install needed tools for bastion (git, ansible, azure) - Depends on VM
    1. Master
        - VMs -  Depends on Master NICs and AS
        - Run master prep script - Depeds on VM
    1. Infra VMs
        - VMs - Depends on Bastion NICs and AS
        - Run node prep script - Depeds on VM
    1. Node VMs
        - VMs - Depends on Bastion NICs and AS
        - Run node prep script - Depeds on VM
7. Deploy Openshift deployment script from bastion VM
