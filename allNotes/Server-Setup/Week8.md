# Server Setup Week 8 (11/22/2021)
## SAMBA
Samba is a file server that enables you to do file sharing across different OS over a network. In this example we will install Samba in Linux and make its directory become accessible in Windows. To install Samba type this following command :
```
sudo yum install samba samba-client samba-common -y
```

after that make a directory in your Linux, For example I named it as `mysamba`. Later we will set this directory as a public shared directory, so we can access its file and edit it in windows.

Next you need to edit smb.conf file

```
vi /etc/samba/smb.conf
```

Add this text at the last line :

```
[mysamba]
    comment = Shared Directory
    path = /mysamba
    browseable = yes
    writeable = yes

    create mode = 0660
    directory mode = 2770

    valid users = root
```

write command `testparm` to check if there is any error. If you see an output line `Loaded services file OK.` it means there is no error and you can continue into the next step. Because you have make a chance into `smb.conf` file then you need to restart Samba by typing command `sudo systemctl restart smb`. Add user into Samba `smbpasswd -a root` after that you need to set its password. Please make sure that you already turn off your firewalld and selinux or else you'll be able to access your directory from Windows.

After all have been set up then you can type your Linux public IP address in Windows search like this `\\192.168.56.107` you'll need to login first. Enter root and password then login. You will able to find `mysamba` directory and root directory. But only root can modify this directory. If you want to add user account and want enable it to edit the directory too, then you need to edit `smb.conf` and change line `valid users = root` into `public = yes` and change the permission by typing `sudo chmod 777 mysamba`.

## Service Type in Linux
There are two types of service in Linux :
1. `xinetd` : it's a agent, when there is a request it will communicate to server so the server will then provide service. But by using xinetd it's time consuming since it need to communicate to server first every time user need a service.
2. `standalone` : this servece will be loaded into system, when there is a request it will run faster since it's already run in the system. But this way it's memory consuming since it's always run in the system.

## COMMAND
### 1. parameter in ping

* `-c [number]` count : will ping for [number] times.

* `-i [number]` : the default is 1 second, you can use this to set interval time between ping

### 2. traceroute
The command `traceroute` will find all router from sender to receiver

### 3. netstat
Used to list all of active network. Usually we will use the command `netstat -tunlp`

* `-t` TCP

* `-u` UDP

* `-n` not resolve

* `-l` listen

* `-p` process

Write command like this :

```
netstat -tunlp | grep 80
```

if you see it print out result, that means this port is open for public

You can also write a command like this to make it return a number :

```
netstat -tunlp | grep 80 | wc -l
```

if it return 0 that means this port is not opened, while number bigger that 0 means it's opened.

In Windows command you can use a similar command by typing :

```
> netstat -an | findstr 80
```