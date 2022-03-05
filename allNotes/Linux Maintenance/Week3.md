# Automatic Operation and Maintenance for Linux System (Week 1 03/02/2022)
## What is Docker
Docker is an open source software platform that allows you to build, test, and deploy applications quickly. It is also a light way of virtualization technique while VM Ware and VM Box, both are heavy way of virtualization because it needs to simulates an entire hardware environment whereas Docker use the isolation technique for virtualization.

## Network Namespace
There are several isolation techniques in Linux including UTS, IPC, Network, PID, Mount, and User isolation. In this part we will try the network namespace isolation with communication between the two namespace shown as the picture below.

<img src="source/(w1)networkNamespace.png" alt="Network Settings" title="Network Settings" width="400"><br>

### 1. creates a network namespace
```
# ip netns add net0
# ip netns add net1
```
Check whether the namespaces have been created succesfully by typing `# ip netns ls`. You should see this output:

```
net0
net1
```

### 2. create veth pair

```
# ip link add type veth
```

### 3. set veth to namespace

```
# ip link set veth0 netns net0
# ip link set veth1 netns net1
```

### 4. change dev name

```
# ip netns exec net0 ip link set dev veth0 name eth0
# ip netns exec net1 ip link set dev veth1 name eth0
```

### 5. assign IP address to veth pair

```
# ip netns exec net0 ip addr add 10.0.1.1/24 brd + dev eth0
# ip netns exec net1 ip addr add 10.0.1.2/24 brd + dev eth0
```

```
# ip netns exec net0 ifconfig eth0 up
# ip netns exec net1 ifconfig eth0 up
```

### 6. try to ping net1 from net0

```
# ip netns exec net0 ping 10.0.1.2
```

if the ping successed then the connection was success

## Install Docker
To install Docker first install the `yum-utils` package and set up the stable repository.

```
# yum install -y yum-utils
# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

After that you cann start to install Docker

```
# yum install docker-ce docker-ce-cli containerd.io
```

Start docker and check for its version to make sure it has been installed correctly

```
# systemctl start docker
# docker -v
```

the output should be something like

```
Docker version 20.10.12, boild e91ed57
```


## Command
#### `# ip netns add [name]`
used to create network namespace

#### `# ip netns ls`
check the list of namespace that you create

#### `# ip netns del [name]`
delete the network namespace with specific name

#### `# ip netns exec [name] ifconfig`
check the network device interface

#### `# ifconfig -a`
show all information network device information even the device that still haven't been activated yet.





What is is isolation technic, one of it sample is network namespace.
what is network namespace


in linux have several virtual technique, one of them is veth pair. you can use command
#ip link add type veth
to create like the image in link telegram

-- from cwhu.medium.com --
a file if we run it, it become container.

repository means the place you put the image file.

docker use so many isolation technique, UTS, IPC, Network, PID, Mount, User.

cgroups used in managing resource in docker

so basicaly if someone ask what technique used in docer, it's namespace and cgroups
--------------------------

you need to remember that in Linux the first PID is SystemD

in docker, you need to at least make docker run 1 job, or else docker will terminated

command
# ip netns add [name]
to crate network namespace

# ip netns ls
check the list of namespace that you create

# ip netns del [name]
to delete the network namespace

# ip netns exec [name] ifconfig

# ip netns exec [name] ip addr
you will see loopback interface

# ip netns exec [name] ifconfig lo up

# ifconfig -a
-a all : to see all information

--docker--
# docker ps
to check running docker

if you exit docker and do ps -a, you'll see it's still running
so you need to do
# docker rm [id]




#ip netns add veth0

#ip link set dev veth0 netns net0 (put veth0 to net1)
#ip link set dev veth1 netns net1
#ip netns exec net0 ifconfig -a (to check if it's already put there)
#ip netns exec net0 ip link set dev veth0 name eth0 (in namespace0, change name from veth0 to eth0)
#ip netns exec net1 ip link set dev veth1 name eth1

#ip netns exec net0 ip addr add 10.0.1.1/24 brd + dev eth0
#ip netns exec net0 ifconfig eth0 up
#ip netns exec net1 ip addr add 10.0.1.2/24 brd + dev eth0
#ip netns exec net1 ifconfig eth0 up

#ip netns exec net0 ping 10.0.1.2

#ip exec netns net1 echo "hi" > hi.htm
create simple webpage in net1

#ip exec netns net1 python -m Simple HTTPServer 80
used to start hosting website in net1

#ip netns exec net0 curl http://10.0.1.2/hi.htm