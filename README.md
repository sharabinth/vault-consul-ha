# Vault Enterprise HA Setup
This repo has a vagrant file to create an enterprise Vault Cluster with Consul backend.  Consists of a Vault cluster with 2 server nodes and a Consul Cluster with 3 server nodes. Vault server includes a Consul client. All servers are set without TLS.

## Pre-Requisites
Create a folder named as ```ent``` and copy the Consul and Vault enterprise binary zip files

```e.g., consul-enterprise_1.4.4+prem_linux_amd64.zip```

## Consul Servers
3 Consul server cluster nodes are created at the following addresses

```
Consul1   10.100.1.11
Consul2   10.100.1.12
Consul3   10.100.1.13
```

One of the Consul servers would become the leader.

## Vault Servers
2 Vault server cluster nodes are created at the following addresses

```
Vault1   10.100.2.11
Vault2   10.100.2.12
```

Vault server nodes do include a Consul client in each node. After setting up, one of the Vault servers will be elected as a leader

## Usage
If the ubuntu box is not available then it will take sometime to download for the first time.  After the the servers can be destroyed and recreated easily with Vagrant

```
$vagrant up

$vagrant status

```

To check the status of the Consul servers ssh into one of the nodes and check the cluster members and identify the leader.

```
$vagrant ssh consul1

vagrant@c1: $consul members

vagrant@c1: $consul operator raft list-peers 

```

## Initialising and Unsealing Vault
```
$vagrant ssh vault1

vagrant@v1: $vault status

vagrant@v1: $vault operator init -key-shares=1 -key-threshold=1

vagrant@v1: $vault operator unseal <unseal-key>

vagrant@v1: $vault status

vagrant@v1: $exit

$vagrant ssh vault1

vagrant@v2: $vault operator unseal <unseal-key>

```

## Accessing UI
One of the Consul server nodes can be used to access the UI on port 8500.  The UI will not work if the leader is not elected.

http://10.100.1.11:8500 

One of the Vault server nodes can be used to access the UI on port 8500

http://10.100.2.11:8200 




