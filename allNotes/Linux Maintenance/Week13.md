# Automatic Operation and Maintenance for Linux System (Week 13 05/25/2022)
## Ansible Command
1. `$ ansible servers -m command -a "ifconfig"`<br>
you can write it as `ansible servers -m -a "ifconfig"`
since the default module is `command`.


2. `$ ansible servers -m shell -a "ifconfig | grep -A 3 enp0s3"`<br>
`-A` : after<br>
shell command support pipe, while command doesn't support it
if you type the above command as
`$ ansible servers -m command -a "ifconfig | grep -A 3 enp0s3"`
it will find error.

3. `$ ansible-doc -l`<br>
show all supported module in ansible

4. `$ ansible-doc -s onyx_ospf`<br>
`-s` summary : show the description briefly (hide the details)

5. `$ ansible server1 -m command -a "uptime"`<br>
will show how many users in this server and how long it has been active

6. `$ ansible -m command -a "free -m" server1`<br>
see the memory usage in server1

7. `$ ansible server1 -m command -a "useradd tom"`<br>
used to add user in server1

8. `$ ansible server1 -m command -a "id peter"`<br>
used to check whether a user is exist in the server. If it show red line output that means the user is not exist, yellow means the user is exist

9. `$ ansible server1 -m command -a "pwd"` <br>
check your working directory when you execute a command in server1. The default path is `/root` directory

10. command with `< > | and & z` should be run using shell module

11. `$ ansible server1 -m command -a "creates=a ls"`<br>
if file a is `not exist`, it will execute ls command, else skipped.

12. `$ ansible server1 -m command -a "removes=b ls"` <br>
if file b is `exist`, it will execute ls command, else the command will be skipped. You can use it like this:<br>
`$ ansible server1 -m command -a "removes=b rm b"`<br>
it will remove file b if it's exist

13. `$ ansible server1 -m command -a "chdir=/tmp creates=b touch b"` <br>
will first change directory to /tmp. If b is not exist then execute `touch b`

14. `$ ansible server1 -m script -a "./a.sh"` <br>
execute a.sh file from current server in server1

15. `$ ansible server1 -m yum -a "name=httpd state=latest"` <br>
used to install package in other server

16. `$ ansible server1 -m shell -a "rpm -qa | grep httpd"` <br>
check whether the module has been installed in server