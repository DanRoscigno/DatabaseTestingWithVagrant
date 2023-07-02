# Database testing with Vagrant

This repository provides a Vagrantfile and configuration files for the virtual machines
used to test the documentation available in
[GitHub](https://github.com/ClickHouse/clickhouse-docs/blob/71ad1697d196d8f983f0b404c77a75dcf0afaff1/docs/en/deployment-guides/replicated.md#testing)
or the
[ClickHouse documentation](https://clickhouse.com/docs/en/architecture/replication#testing).

This procedure is for RPM based systems, other package formats should be similar.

## Basic setup of VirtualBox and Vagrant

For Fedora host machine see the
[Fedora Project](https://developer.fedoraproject.org/tools/vagrant/vagrant-libvirt.html)

- install Oracle VirtualBox-use the RPM from the Oracle site, not the Fedora repository:

```shell
sudo dnf install ./VirtualBox-7.0-7.0.8_156879_fedora36-1.x86_64.rpm
```

- install vagrant-use the RPM from the vagrant site, not the Fedora repository:

```shell
sudo dnf install ~/Downloads/vagrant-2.3.6-1.x86_64.rpm
```

- add package for hosts file sync

```shell
vagrant plugin install vagrant-hosts
```

- any problems with creating a box or starting a box restart libvirt:

```shell
sudo service libvirtd restart
```

## Create working dir and init base image

- Initialize a Debian 12 (bookworm) box configuration

```shell
mkdir -p vagrant/clickhouse/replication
cd vagrant/clickhouse/replication
vagrant init debian/bookworm64
```

- Run the preceding box

```shell
# note, run this from the above vagrant/clickhouse/replication dir
vagrant up
```

- ssh in

```shell
# If you see an error related to mounting a disk or NFS then you may need
# an updated version of Vagrant and Virtualbox, see the **Basic install**
section and download the RPMs or debs.
vagrant ssh
```

## Tests and raw history

Tests are available in
[GitHub](https://github.com/ClickHouse/clickhouse-docs/blob/71ad1697d196d8f983f0b404c77a75dcf0afaff1/docs/en/deployment-guides/replicated.md#testing)
or the
[ClickHouse documentation](https://clickhouse.com/docs/en/architecture/replication#testing)

```bash
vagrant up clickhouse-keeper-01
vagrant up clickhouse-keeper-02
vagrant up clickhouse-keeper-03
vagrant up clickhouse-01
vagrant up clickhouse-02
vagrant ssh clickhouse-keeper-01 "sudo systemctl start clickhouse-keeper"
vagrant ssh clickhouse-keeper-01 -c "sudo systemctl start clickhouse-keeper"
vagrant ssh clickhouse-keeper-02 -c "sudo systemctl start clickhouse-keeper"
vagrant ssh clickhouse-keeper-03 -c "sudo systemctl start clickhouse-keeper"
vagrant ssh clickhouse-keeper-03 -c "nc localhost 9181"
vagrant ssh clickhouse-keeper-03
vagrant ssh clickhouse-01 -c "sudo systemctl start clickhouse-server"
vagrant ssh clickhouse-02 -c "sudo systemctl start clickhouse-server"
vagrant ssh clickhouse-02 -c "clickhouse-client"
vagrant ssh clickhouse-01 -c "clickhouse-client"
```
