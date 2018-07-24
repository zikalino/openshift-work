# Openshift work

This is temporary repo to do experiments with openshift

## Requirements

Make sure you have ansible 2.6.1, and make sure you have azure cli 2.41. Also, make sure you update azure for pip, especially if you run into not found element errors.

```bash
sudo pip install ansible[azure]
```

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