# Deploying docker-based CEPH cluster using ceph-ansible
###__Why Docker and Ceph?

● Stateless containers
● Stateful data
● Location independent access
● Robust access protocols
● Massively scalable
● Open source

####__Install ansible
For installing ansible and configuring secret-free login between nodes, this is not covered here. Please refer to Official Documents at https://docs.ansible.com/
__Releases
The following branches should be used depending on your requirements. The stable-* branches have been QE tested and sometimes recieve backport fixes throughout their lifecycle. The master branch should be considered experimental and used with caution.

stable-3.0 Supports Ceph versions jewel and luminous. This branch requires Ansible version 2.4.
stable-3.1 Supports Ceph versions luminous and mimic. This branch requires Ansible version 2.4.
stable-3.2 Supports Ceph versions luminous and mimic. This branch requires Ansible version 2.6.
stable-4.0 Supports Ceph version nautilus. This branch requires Ansible version 2.8.
stable-5.0 Supports Ceph version octopus. This branch requires Ansible version 2.9.
master Supports the master branch of Ceph. This branch requires Ansible version 2.9.

__Download ceph-ansible
```python
git clone https://github.com/ceph/ceph-ansible.git
cd ceph-ansible
# Refer to the official document of ceph-ansible http://docs.ceph.com/ceph-ansible/master for branch instructions.
# stable-3.1 Support for Ceph version luminous and mimic. This branch supports Ansible version 2.4.
git checkout stable-3.1
```

__Configure ceph-ansible
```python
cp group_vars/all.yml.sample group_vars/all.yml
cp group_vars/osds.yml.sample group_vars/osds.yml
cp site-docker.yml.sample site-docker.yml
```

__Edit the group_vars/all.yml file. The following information should be adapted according to the actual situation.
```python
generate_fsid: true
monitor_interface: eth0
journal_size: 5120
public_network: 10.0.0.0/24
cluster_network: 10.0.0.0/24
ceph_docker_image: "ceph/daemon"
ceph_docker_image_tag: latest-mimic
containerized_deployment: true
ceph_docker_registry: docker.io
radosgw_interface: eth0
```

__Edit the group_vars/osds.yml file. The following information should be adapted according to the actual situation.

```python
osd_scenario: collocated
devices:
  - /dev/xvdb
  - /dev/xvdc
  - /dev/xvdc
```

__Edit the hosts file /etc.ansible/hosts, the following information should be adapted according to the actual situation.
```python
[mons]
ip-10-0-0-80.ec2.internal

[mgrs]
ip-10-0-0-80.ec2.internal

[osds]
ip-10-0-0-80.ec2.internal 

[rgws]
ip-10-0-0-80.ec2.internal 
```

__Start deployment
If the following command fails, the log file defaults to / var/log/ansible.log, you can refer to the error log for processing. (Note: During deployment, docker will be installed by default on each node. This step script does not determine whether the current machine has docker installed. If the target machine has docker installed, there may be errors. The solution is to comment out the corresponding steps to install docker. The corresponding contents are roles/ceph-docker-common/tasks/pre_requisites/pre requisites. Ites.yml.)
```python
ansible-playbook site-docker.yml
```
__Output:

```python
docker ps
docker exec d77bd7f96bfe ceph -s
```
![alt text](https://github.com/ajinkyaingole30/Readme/blob/master/Screenshot%20(216).png?raw=true)

I hope that you have understood how to deploy ceph in docker using ansible.

### Happy Cephing!!
