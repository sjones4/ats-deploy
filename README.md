# Ansible playbook for Eucalyptus Cloud deployment

## Deploying Eucalyptus Cloud

Create an inventory for your environment:

```
cp inventory_example.yml inventory.yml
vi inventoy.yml
```

to install with EDGE network mode:

```
ansible-playbook -i inventory.yml playbook[_edge].yml
```

to install with VPCMIDO network mode:

```
ansible-playbook -i inventory.yml playbook_vpcmido.yml
```

to remove a eucalyptus installation and main dependencies:

```
ansible-playbook -i inventory.yml playbook_clean.yml
```

Tags can be used to control which aspects of the playbook are used:

* `image` : `packages` and generic configuration
* `packages` : installs yum repositories and rpms

Example tag use:

```
ansible-playbook --tags      image -i inventory.yml playbook.yml
ansible-playbook --skip-tags image -i inventory.yml playbook.yml
```

which would run the playbook in two parts, the first installing packages
and non-deployment specific configuration, and the second completing the
deployment.

## Maintaining Eucalyptus Cloud

Maintenance playbooks support operation of a previously deployed cloud.

* Update to the latest release

### Eucalyptus update

Updating a deployment will install the latest Eucalyptus packages and
system images.

The inventory used should be the same as the current deployment you
cannot modify the deployment as part of the update.

To update to the latest release:

```
ansible-playbook -i inventory.yml playbook_update.yml
```

post update you will need to `delete` and `create` system stacks if
needed for the imaging worker and/or for a console cloud deployment.
