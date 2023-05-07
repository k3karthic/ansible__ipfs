# Ansible — IPFS

The Ansible playbook in this repository installs [Kubo](https://github.com/ipfs/kubo) an Ubuntu 22.04 instance.

**Assumption:** The instance runs in Oracle Cloud using the Terraform script below,
* terraform__oci-instance-2
	* GitHub: [github.com/k3karthic/terraform__oci-instance-2](https://github.com/k3karthic/terraform__oci-instance-2)
	* Codeberg: [codeberg.org/k3karthic/terraform__oci-instance-2](https://codeberg.org/k3karthic/terraform__oci-instance-2)

## Code Mirrors

* GitHub: [github.com/k3karthic/ansible__ipfs](https://github.com/k3karthic/ansible__ipfs/)
* Codeberg: [codeberg.org/k3karthic/ansible__ipfs](https://codeberg.org/k3karthic/ansible__ipfs)

## Requirements

Install the following before running the playbook,
```
pip install oci
ansible-galaxy collection install oracle.oci
```

## Dynamic Inventory

The Oracle [Ansible Inventory Plugin](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/ansibleinventoryintro.htm) populates public Ubuntu instances.

The target Ubuntu instance must have the freeform tag `ipfs_service: yes`.

## Configuration

1. Update `inventory/oracle.oci.yml`,
    1. Specify the region where you have deployed your server on Oracle Cloud. List of regions are at [docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm](https://docs.oracle.com/en-us/iaas/Content/General/Concepts/regions.htm).
    1. Configure the authentication as per the [Oracle Guide](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm#SDK_and_CLI_Configuration_File)
1. Set username and ssh authentication in `inventory/group_vars/`
1. Set the `ipfs_version`

## Deployment

Run the playbook using the following command,
```
./bin/apply.sh
```

## Encryption

Encrypt sensitive files (SSH private keys and `inventory/group_vars/tag_ipfs_service=yes.yml`) before saving them. `.gitignore` must contain the unencrypted file paths.

Use the following command to decrypt the files after cloning the repository,

```
$ ./bin/decrypt.sh
```

Use the following command after running terraform to update the encrypted files,

```
$ ./bin/encrypt.sh <gpg key id>
```
