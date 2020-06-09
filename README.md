Role Name
=========

Kodo Cloud is a backup solution for Office 365. This role deploys Kodo Cloud Server 
which is a central point of management.

Requirements
------------

CentOS/RHEL 8 minimal installation and public key authentication between command host and target machine

Role Variables
--------------

Defaults:
```
mariadb_version: "10.4"
mariadb_distro: "centos{{ ansible_distribution_major_version }}-amd64"
mariadb_repo_url: "http://yum.mariadb.org/{{ mariadb_version }}/{{ mariadb_distro }}"
mariadb_repo_gpg_key: "https://yum.mariadb.org/RPM-GPG-KEY-MariaDB"
kodo_repo: "http://repo.storware.eu/kodo-cloud/current/el{{ ansible_distribution_major_version }}"
kodo_staging_path: "/kodo_data"
kodo_backup_destination_path: "{{ kodo_staging_path }}/backups"
vdo_volume_name: "kodo"
vdo_fs: "xfs"
vdo_fs_mkfs_opts: "-K"
vdo_mount_point: "{{ kodo_staging_path }}"
```

* `mariadb_*` - responsible for MariaDB installation (repository, version, distribution)
* `kodo_repo` - points to Kodo Cloud RPM repository
* `kodo_staging_path` - allows to configure where staging space resides on the server
* `kodo_backup_destination_path` - allows to configure where backup storage resides on the server
* `vdo_physical_device` - **by default not defined** - when set, configures VDO on the block device that has been specified (such as `/dev/sdb`)
* `vdo_logical_device_size` - logical size of VDO volume, by default 3 * size of physical size of the VDO block device
* `vdo_volume_name` - if VDO is to be configured - name of the VDO volume
* `vdo_fs` - if VDO is to be configured - type of effective file system created on top of VDO
* `vdo_fs_mkfs_opts` - if VDO is to be configured - mkfs parameters when creating effective file system on top of VDO
* `vdo_mount_point` - if VDO is to be configured - mount point for effective file system used on top of VDO - usually staging space together with backup destination


Dependencies
------------

N/A

Example Playbook
----------------

This deploys Kodo server on `server` host (only one server can be deployed)
and multiple agents on `agents` hosts

```
- hosts: server
  roles:
   - xe0nic.ansible_kodo_cloud_server

- hosts: agents
  roles:
   - xe0nic.ansible_kodo_cloud_agent
```

Example hosts inventory (you need to make sure that SSH public key authentication for
ansible user provided in inventory is configured):

```
[all:vars]
ansible_user = root

[server]
192.168.155.233

[agents]
192.168.155.233 agent_name=agent1
```

License
-------

MIT

Author Information
------------------

For more information visit product website: https://storware.eu/products/kodo-for-cloud
Documentation: https://storware.gitbook.io/kodo-for-cloud-office365