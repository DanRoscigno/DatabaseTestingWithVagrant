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

## Tests

### Start and test ClickHouse Keeper

This test starts the Keeper virtual machines and then the Keeper process on each machine.
The `echo mntr | nc` commands use ZooKeeper four-letter words to retrieve the status of
the Keeper servers. Look for one Keeper server to have `zk_server_state	leader` and the 
other two to have `follower` status.

```shell
vagrant up clickhouse-keeper-01
vagrant up clickhouse-keeper-02
vagrant up clickhouse-keeper-03

vagrant ssh clickhouse-keeper-01 -c "sudo systemctl start clickhouse-keeper"
vagrant ssh clickhouse-keeper-02 -c "sudo systemctl start clickhouse-keeper"
vagrant ssh clickhouse-keeper-03 -c "sudo systemctl start clickhouse-keeper"

vagrant ssh clickhouse-keeper-01 -c "echo mntr | nc localhost 9181"
vagrant ssh clickhouse-keeper-02 -c "echo mntr | nc localhost 9181"
vagrant ssh clickhouse-keeper-03 -c "echo mntr | nc localhost 9181"
```

### Start and test ClickHouse server

```bash
vagrant up clickhouse-01
vagrant up clickhouse-02
vagrant ssh clickhouse-01 -c "sudo systemctl start clickhouse-server"
vagrant ssh clickhouse-02 -c "sudo systemctl start clickhouse-server"
```

### Functional tests

Tests are available in
[GitHub](https://github.com/ClickHouse/clickhouse-docs/blob/71ad1697d196d8f983f0b404c77a75dcf0afaff1/docs/en/deployment-guides/replicated.md#testing)
or the
[ClickHouse documentation](https://clickhouse.com/docs/en/architecture/replication#testing)

Open a shell on each virtual machine `clickhouse-01` and `clickhouse-02` and run the functional tests.

```shell
vagrant ssh clickhouse-01 -c clickhouse-client
vagrant ssh clickhouse-02 -c clickhouse-client
```
