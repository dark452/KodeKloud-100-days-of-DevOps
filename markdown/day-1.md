# Day 1

To accommodate the backup agent tool's specifications, the system admin team at `xFusionCorp Industries` requires the creation of a user with a non-interactive shell. Here's your task:

Create a user named `rose` with a non-interactive shell on `App Server 2`.

Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

```bash
[steve@stapp02 ~]$ sudo adduser -s /sbin/nologin rose

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 

[steve@stapp02 ~]$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:998:996:User for polkitd:/:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/proc:/sbin/nologin
pipewire:x:997:995:PipeWire System Daemon:/run/pipewire:/usr/sbin/nologin
geoclue:x:996:994:User for geoclue:/var/lib/geoclue:/sbin/nologin
flatpak:x:995:993:User for flatpak system helper:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
steve:x:1000:1000::/home/steve:/bin/bash
rose:x:1001:1001::/home/rose:/sbin/nologin
`

CONGRATULATIONS!!!!
You have successfully completed the challenge.Results have been saved. Ref ID:680216e85500bdf7ab9a7af8


That might have been an easy task for you. But don't worry. Depending on the tasks you complete, you will get more difficult and advanced tasks assigned in the future.

You can also view your results in your dashboard under the "Active Practice" page.