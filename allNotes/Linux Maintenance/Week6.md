# Automatic Operation and Maintenance for Linux System (Week 6 03/23/2022)

## Backup file into another device

use this command to back up the file into another device

```
# rsync -avzh testdir/ user@192.168.56.107:/tmp/backup
```

But this way we need to enter password each time we want to do the back up. Since our goal is to enable it to do it automaticaly, so we need to do this several steps

Generate a key and copy it to your other device

```
# ssh-keygen
# ssh-copy-id user@ipaddress
```

After that you can execute `# rsync -avzh testdir/ user@192.168.56.107:/tmp/backup` without entering the password.

## Automaticaly backup file into other device
For example we have 2 device, we set `192.168.56.107` as our server and `192.168.56.108` as the client. If we want to backup the file from server to client and make it able to synchronize automathicaly first in server device we need to make some change into rsyncd.conf `# gedit /etc/rsyncd.conf` by adding this several file

```
uid=root
gid=root
pid file = /var/run/rsyncd.pid
log file = /var/log/rsyncd.log
secrets file = /etc/rsyncd.passwd

[mod1]
   path = /backup1
   read only = no
   auth users = vuser
```

create `backup1` directory in root directory (`# mkdir backup1`) and add new file to write the password inside it `# gedit /etc/rsyncd.passwd`

```
vuser:123456
```

don't forget to change its file permission by executing `# chmod 600 /etc/rsyncd.passwd` and start the rsyncd `# systemctl start rsyncd`

Before do the synchronization you need to synchronize both of the device's time by executing `# ntpdate watch.stdtime.gov.tw` in both device. Note that `watch.stdtime.gov.tw` is used for Taiwan.

While at client device, first you need to create a new file to put the account password `# gedit /etc/rsync_vuser.passwd` and put `123456` in it. Don't forget to change the file permission by executing `# chmod 600 /etc/rsync_vuse.passwd`

after that you can execute this command in client :

```
# rsync -avz --progress --password-file=/etc/rsync_vuser.passwd testdir/ vuser@192.168.56.107::mod1
```

---

Other Notes :

basicaly ssh served in port 22. But, what if it is served on another port e.x. port 123, then you need to execute the following command :

`# rsync -avzh -e "ssh -p 123" testdir user@ipaddress:/tmp/backup/`

you also can exclude several file from rsync :

`# rsync -avzh --exclude 'a.txt' --exclude 'b.txt' testdir/ user@ipaddress:/tmp/backup/`

or

`# rsync -avzh --exclude '*.txt' testdir/ user@ipaddress:/tmp/backup/`

to exclude specific file type. You also can backup for specific file with have specific size by defining its `--min-size` and `--max-size`.


for inotify
https://segmentfault.com/a/1190000018096553
basicaly when we need to backup we should do it manualy, but we wat to do automaticaly. using inotify to monitor whether there is any change or not.

## COMMAND
#### `# rsync`
used for backup files. basically will copy the file from local to remote.

```
rsync [option] [source] [destination]
```

example :
```
rsync -avh /path/to/myfolder /home/pi/tmp
```

flags an parameters :

`-v`: variable:will show more info

`-z` : will compress file

`--progress` : show the progress

#### `# rsync --delete`
basically if you delete file localy, it will not delete the file on remote. if you also want to delete it remotely, you need to add --delete.