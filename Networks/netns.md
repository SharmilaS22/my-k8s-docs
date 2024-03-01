

namespace in network - allows us to separate the processes in each container and host

to create a network namespace ns

```sh
ip netns add <netns-name>
ip netns add dev
```
The `ip link` command displays the network interfaces in the system
The containers run in separate namespace and does not have visibility on networkk configurations of host or other containers.

To display network interfaces in a particular namespace
```sh
ip -n red link 

#or

ip netns exec red ip link
ip netns exec <ns-name> <commands..>
```

The container has its own virtual eni and arp table and route table maintained separately from hosts/other containers.


ARP table 
    - dynamically stores mapping of ip address to MAC address of devices in the local network
    - use `arp -a` command to view the arp table values in the network

The network namespaces - can be connected using veth (veth - virtual eth, also called 'cable')
```sh
# create network namespace - app and db
ip netns add app-ns
ip netns add db-ns

# create veth for app and db netns'
ip link add veth-app type veth peer name veth-db

# add the veth created to correspoding netns
ip link set veth-app netns app-ns
ip link set veth-db netns db-ns

# assign ip add to the netns
# ip -n <ns-name> addr add <ip> dev <veth>
ip -n app-ns addr add 192.168.1.0 dev veth-app
ip -n db-ns addr add 192.168.1.1 dev veth-db

ip -n app-ns link set veth-app up
ip -n db-ns link set veth-db up

# to check connectivity between two ns
ip netns exec app-db ping 192.168.1.1 # db-ns' ip

# The ip:mac mapping is added to arp table of both namespaces for the other one.

```
