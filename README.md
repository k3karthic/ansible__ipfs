# Ansible - IPFS

The Ansible playbook in this repository installs [go-ipfs](https://github.com/ipfs/go-ipfs) on an Ubuntu 20.04 instance and configures an [Oracle Object Storage](https://www.oracle.com/cloud/storage/object-storage.html) bucket as the datastore using the [go-ds-s3](https://github.com/ipfs/go-ds-s3) plugin.

**Assumption:** The instance runs in Oracle Cloud using the terraform script below,
* terraform__oci-instance-2
	* GitHub: [github.com/k3karthic/terraform__oci-instance-2](https://github.com/k3karthic/terraform__oci-instance-2).
	* Codeberg: [codeberg/k3karthic/terraform__oci-instance-2](https://codeberg.org/k3karthic/terraform__oci-instance-2).

The Terraform script below creates a bucket and associated credentials (customer secret key),
* terraform__oci-storage-s3
	* GitHub: [github.com/k3karthic/terraform__oci-storage-s3](https://github.com/k3karthic/terraform__oci-storage-s3).
	* Codeberg: [codeberg.org/k3karthic/terraform__oci-storage-s3](https://codeberg.org/k3karthic/terraform__oci-storage-s3).

The playbook expects the ipfs binary to be placed in the `resources` folder. A terraform script to build the appropriate binary can be found at [https://github.com/k3karthic/terraform__ipfs-build-s3](https://github.com/k3karthic/terraform__ipfs-build-s3).

## Dynamic Inventory

The Oracle [Ansible Inventory Plugin](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/ansibleinventoryintro.htm) populates public Ubuntu instances.

The target Ubuntu instances must have the freeform tag `ipfs_service: yes`.

## Requirements

Install the following before running the playbook,
```
pip install oci
ansible-galaxy collection install oracle.oci
```

## Playbook Configuration

1. Update `inventory/oracle.oci.yml`,
    1. Specify the region where you have deployed your server on Oracle Cloud.
    1. Configure the authentication as per the [Oracle Guide](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/sdkconfig.htm#SDK_and_CLI_Configuration_File).
1. Set username and ssh authentication in `inventory/group_vars/`.
2. Create `inventory/group_vars/tag_ipfs_service=yes.yml` based on the sample `inventory/group_vars/tag_ipfs_service=yes.yml.sample`.

## Deployment

Run the playbook using the following command,
```
./bin/apply.sh
```

## Encryption

Encrypt sensitive files (SSH private keys) before saving them. `.gitignore` must contain the unencrypted file paths.

Use the following command to decrypt the files after cloning the repository,

```
$ ./bin/decrypt.sh
```

Use the following command after running terraform to update the encrypted files,

```
$ ./bin/encrypt.sh <gpg key id>
```
