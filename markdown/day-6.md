# Day 6: Create a Cron Job

The `Nautilus` system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in `Stratos DC` on a set schedule. Before that they need to test similar functionality with a sample cron job. Therefore, perform the steps below:

1. Install `cronie` package on all `Nautilus` app servers and start `crond` service.
2. Add a cron `*/5 * * * * echo hello > /tmp/cron_text` for `root` user.


AppServer 01
```bash
thor@jump-host ~$ ssh stapp01 -ltony
The authenticity of host 'stapp01 (10.244.235.30)' can't be established.
ED25519 key fingerprint is SHA256:4b4Zl3A4n/NI6X+ygZdrwWKU1QE7RPpmc/ZjVN3vLn8.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01' (ED25519) to the list of known hosts.
tony@stapp01's password: 
[tony@stapp01 ~]$ sudo -i

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
[root@stapp01 ~]# dnf install cronie
CentOS Stream 9 - BaseOS                      4.9 MB/s | 9.0 MB     00:01    
CentOS Stream 9 - AppStream                   553 kB/s |  28 MB     00:52    
CentOS Stream 9 - Extras packages              25 kB/s |  21 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_6  50 MB/s |  21 MB     00:00    
Extra Packages for Enterprise Linux 9 openh26 3.5 kB/s | 2.5 kB     00:00    
Extra Packages for Enterprise Linux 9 - Next  428 kB/s | 265 kB     00:00    
Dependencies resolved.
==============================================================================
 Package            Arch       Version                       Repository  Size
==============================================================================
Installing:
 cronie             x86_64     1.5.7-16.el9                  baseos     119 k
Installing dependencies:
 cronie-anacron     x86_64     1.5.7-16.el9                  baseos      31 k
 crontabs           noarch     1.11-26.20190603git.el9       baseos      19 k

Transaction Summary
==============================================================================
Install  3 Packages

Total download size: 168 k
Installed size: 367 k
Is this ok [y/N]: y
Downloading Packages:
(1/3): crontabs-1.11-26.20190603git.el9.noarc 151 kB/s |  19 kB     00:00    
(2/3): cronie-anacron-1.5.7-16.el9.x86_64.rpm 234 kB/s |  31 kB     00:00    
(3/3): cronie-1.5.7-16.el9.x86_64.rpm         660 kB/s | 119 kB     00:00    
------------------------------------------------------------------------------
Total                                         239 kB/s | 168 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                      1/1 
  Installing       : cronie-anacron-1.5.7-16.el9.x86_64                   1/3 
  Running scriptlet: cronie-anacron-1.5.7-16.el9.x86_64                   1/3 
  Installing       : cronie-1.5.7-16.el9.x86_64                           2/3 
  Running scriptlet: cronie-1.5.7-16.el9.x86_64                           2/3 
Created symlink /etc/systemd/system/multi-user.target.wants/crond.service → /usr/lib/systemd/system/crond.service.

  Installing       : crontabs-1.11-26.20190603git.el9.noarch              3/3 
  Running scriptlet: crontabs-1.11-26.20190603git.el9.noarch              3/3 
  Verifying        : cronie-1.5.7-16.el9.x86_64                           1/3 
  Verifying        : cronie-anacron-1.5.7-16.el9.x86_64                   2/3 
  Verifying        : crontabs-1.11-26.20190603git.el9.noarch              3/3 

Installed:
  cronie-1.5.7-16.el9.x86_64               cronie-anacron-1.5.7-16.el9.x86_64 
  crontabs-1.11-26.20190603git.el9.noarch 

Complete!
[root@stapp01 ~]# systemctl status cronie
Unit cronie.service could not be found.
[root@stapp01 ~]# systemctl status cron
Unit cron.service could not be found.
[root@stapp01 ~]# systemctl status crond
○ crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: >
     Active: inactive (dead)
[root@stapp01 ~]# systemctl start crond
[root@stapp01 ~]# systemctl enabled crond
Unknown command verb enabled.
[root@stapp01 ~]# systemctl enable crond
[root@stapp01 ~]# systemctl status crond
● crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: >
     Active: active (running) since Wed 2026-07-15 05:23:39 UTC; 17s ago
   Main PID: 6620 (crond)
      Tasks: 1 (limit: 404712)
     Memory: 960.0K
        CPU: 2ms
     CGroup: /system.slice/crond.service
             └─6620 /usr/sbin/crond -n

Jul 15 05:23:39 stapp01 systemd[1]: Started Command Scheduler.
Jul 15 05:23:39 stapp01 crond[6620]: (CRON) STARTUP (1.5.7)
Jul 15 05:23:39 stapp01 crond[6620]: (CRON) INFO (Syslog will be used instead>
Jul 15 05:23:39 stapp01 crond[6620]: (CRON) INFO (RANDOM_DELAY will be scaled>
Jul 15 05:23:39 stapp01 crond[6620]: (CRON) INFO (running with inotify suppor>
[root@stapp01 ~]# crontab -e
no crontab for root - using an empty one
crontab: installing new crontab
[root@stapp01 ~]# crontab -l
*/5 * * * * echo hello > /tmp/cron_text
[root@stapp01 ~]# ls -larth /tmp/
.ICE-unix/
.X11-unix/
.XIM-unix/
.font-unix/
cron_text
systemd-private-240069a991c9455fb194bc6f4a85607d-dbus-broker.service-a6AmWU/
systemd-private-240069a991c9455fb194bc6f4a85607d-rtkit-daemon.service-aHje4s/
systemd-private-240069a991c9455fb194bc6f4a85607d-systemd-logind.service-sDBDEa/
systemd-private-240069a991c9455fb194bc6f4a85607d-upower.service-rgK2ub/
[root@stapp01 ~]# ls -larth /tmp/cron_text 
-rw-r--r-- 1 root root 6 Jul 15 05:25 /tmp/cron_text
[root@stapp01 ~]# cat /tmp/cron_text 
hello
```

AppServer 02

```bash
thor@jump-host ~$ ssh stapp02 -lsteve
The authenticity of host 'stapp02 (10.244.164.73)' can't be established.
ED25519 key fingerprint is SHA256:G9KzlzZztu83O6MtHIFMEVq7rKoTxFGJpFgFNFkB/Xk.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp02' (ED25519) to the list of known hosts.
steve@stapp02's password: 
[steve@stapp02 ~]$ sudo -i

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for steve: 
[root@stapp02 ~]# dnf install cronie
CentOS Stream 9 - BaseOS                      9.2 MB/s | 9.0 MB     00:00    
CentOS Stream 9 - AppStream                    10 MB/s |  28 MB     00:02    
CentOS Stream 9 - Extras packages              28 kB/s |  21 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_6  20 MB/s |  21 MB     00:01    
Extra Packages for Enterprise Linux 9 openh26 2.2 kB/s | 2.5 kB     00:01    
Extra Packages for Enterprise Linux 9 - Next  1.5 MB/s | 265 kB     00:00    
Dependencies resolved.
==============================================================================
 Package            Arch       Version                       Repository  Size
==============================================================================
Installing:
 cronie             x86_64     1.5.7-16.el9                  baseos     119 k
Installing dependencies:
 cronie-anacron     x86_64     1.5.7-16.el9                  baseos      31 k
 crontabs           noarch     1.11-26.20190603git.el9       baseos      19 k

Transaction Summary
==============================================================================
Install  3 Packages

Total download size: 168 k
Installed size: 367 k
Is this ok [y/N]: y
Downloading Packages:
(1/3): cronie-anacron-1.5.7-16.el9.x86_64.rpm 297 kB/s |  31 kB     00:00    
(2/3): crontabs-1.11-26.20190603git.el9.noarc 165 kB/s |  19 kB     00:00    
(3/3): cronie-1.5.7-16.el9.x86_64.rpm         719 kB/s | 119 kB     00:00    
------------------------------------------------------------------------------
Total                                         564 kB/s | 168 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                      1/1 
  Installing       : cronie-anacron-1.5.7-16.el9.x86_64                   1/3 
  Running scriptlet: cronie-anacron-1.5.7-16.el9.x86_64                   1/3 
  Installing       : cronie-1.5.7-16.el9.x86_64                           2/3 
  Running scriptlet: cronie-1.5.7-16.el9.x86_64                           2/3 
Created symlink /etc/systemd/system/multi-user.target.wants/crond.service → /usr/lib/systemd/system/crond.service.

  Installing       : crontabs-1.11-26.20190603git.el9.noarch              3/3 
  Running scriptlet: crontabs-1.11-26.20190603git.el9.noarch              3/3 
  Verifying        : cronie-1.5.7-16.el9.x86_64                           1/3 
  Verifying        : cronie-anacron-1.5.7-16.el9.x86_64                   2/3 
  Verifying        : crontabs-1.11-26.20190603git.el9.noarch              3/3 

Installed:
  cronie-1.5.7-16.el9.x86_64               cronie-anacron-1.5.7-16.el9.x86_64 
  crontabs-1.11-26.20190603git.el9.noarch 

Complete!
[root@stapp02 ~]# systemctl start crond
[root@stapp02 ~]# systemctl enable crond
[root@stapp02 ~]# systemctl status crond
● crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: >
     Active: active (running) since Wed 2026-07-15 05:24:46 UTC; 13s ago
   Main PID: 6762 (crond)
      Tasks: 1 (limit: 404712)
     Memory: 952.0K
        CPU: 2ms
     CGroup: /system.slice/crond.service
             └─6762 /usr/sbin/crond -n

Jul 15 05:24:46 stapp02 systemd[1]: Started Command Scheduler.
Jul 15 05:24:46 stapp02 crond[6762]: (CRON) STARTUP (1.5.7)
Jul 15 05:24:46 stapp02 crond[6762]: (CRON) INFO (Syslog will be used instead>
Jul 15 05:24:46 stapp02 crond[6762]: (CRON) INFO (RANDOM_DELAY will be scaled>
Jul 15 05:24:46 stapp02 crond[6762]: (CRON) INFO (running with inotify suppor>
[root@stapp02 ~]# crontab -e
no crontab for root - using an empty one
crontab: installing new crontab
[root@stapp02 ~]# crontab -l
*/5 * * * * echo hello > /tmp/cron_text
[root@stapp02 ~]# cat /tmp/cron_text 
cat: /tmp/cron_text: No such file or directory
[root@stapp02 ~]# cat /tmp/cron_text 
cat: /tmp/cron_text: No such file or directory
[root@stapp02 ~]# cat /tmp/cron_text 
cat: /tmp/cron_text: No such file or directory
[root@stapp02 ~]# ls -alrth /tmp/
total 40K
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .font-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .XIM-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .X11-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .ICE-unix
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-rtkit-daemon.service-UcHwEO
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-systemd-logind.service-KUGF6A
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-upower.service-40exnL
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-dbus-broker.service-V38aQ8
drwxrwxrwt 10 root root 4.0K Jul 15 05:25 .
dr-xr-xr-x  1 root root 4.0K Jul 15 05:28 ..
[root@stapp02 ~]# date
Wed Jul 15 05:29:26 UTC 2026
[root@stapp02 ~]# ls -alrth /tmp/
total 40K
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .font-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .XIM-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .X11-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .ICE-unix
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-rtkit-daemon.service-UcHwEO
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-systemd-logind.service-KUGF6A
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-upower.service-40exnL
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-dbus-broker.service-V38aQ8
drwxrwxrwt 10 root root 4.0K Jul 15 05:25 .
dr-xr-xr-x  1 root root 4.0K Jul 15 05:29 ..
[root@stapp02 ~]# ls -alrth /tmp/
total 44K
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .font-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .XIM-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .X11-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:10 .ICE-unix
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-rtkit-daemon.service-UcHwEO
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-systemd-logind.service-KUGF6A
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-upower.service-40exnL
drwx------  3 root root 4.0K Jul 15 05:10 systemd-private-c3e880158dfe481c897ec6163084711f-dbus-broker.service-V38aQ8
-rw-r--r--  1 root root    6 Jul 15 05:30 cron_text
drwxrwxrwt 10 root root 4.0K Jul 15 05:30 .
dr-xr-xr-x  1 root root 4.0K Jul 15 05:30 ..
```

AppServer 03

```bash
thor@jump-host ~$ ssh stapp03 -lbanner
The authenticity of host 'stapp03 (10.244.240.172)' can't be established.
ED25519 key fingerprint is SHA256:aGHp9gNgWCZ8qW9MzAWwsq/sgqBMxwKLD9fIVgnw2j4.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03' (ED25519) to the list of known hosts.
banner@stapp03's password: 
[banner@stapp03 ~]$ sudo -i

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
[root@stapp03 ~]# dnf install cronie
Last metadata expiration check: 0:02:25 ago on Wed Jul 15 05:24:37 2026.
Dependencies resolved.
=============================================================================================
 Package                Architecture   Version                          Repository      Size
=============================================================================================
Installing:
 cronie                 x86_64         1.5.7-16.el9                     baseos         119 k
Installing dependencies:
 cronie-anacron         x86_64         1.5.7-16.el9                     baseos          31 k
 crontabs               noarch         1.11-26.20190603git.el9          baseos          19 k

Transaction Summary
=============================================================================================
Install  3 Packages

Total download size: 168 k
Installed size: 367 k
Is this ok [y/N]: y
Downloading Packages:
(1/3): crontabs-1.11-26.20190603git.el9.noarch.rpm            79 kB/s |  19 kB     00:00    
(2/3): cronie-anacron-1.5.7-16.el9.x86_64.rpm                119 kB/s |  31 kB     00:00    
(3/3): cronie-1.5.7-16.el9.x86_64.rpm                        303 kB/s | 119 kB     00:00    
---------------------------------------------------------------------------------------------
Total                                                        197 kB/s | 168 kB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                     1/1 
  Installing       : cronie-anacron-1.5.7-16.el9.x86_64                                  1/3 
  Running scriptlet: cronie-anacron-1.5.7-16.el9.x86_64                                  1/3 
  Installing       : cronie-1.5.7-16.el9.x86_64                                          2/3 
  Running scriptlet: cronie-1.5.7-16.el9.x86_64                                          2/3 
Created symlink /etc/systemd/system/multi-user.target.wants/crond.service → /usr/lib/systemd/system/crond.service.

  Installing       : crontabs-1.11-26.20190603git.el9.noarch                             3/3 
  Running scriptlet: crontabs-1.11-26.20190603git.el9.noarch                             3/3 
  Verifying        : cronie-1.5.7-16.el9.x86_64                                          1/3 
  Verifying        : cronie-anacron-1.5.7-16.el9.x86_64                                  2/3 
  Verifying        : crontabs-1.11-26.20190603git.el9.noarch                             3/3 

Installed:
  cronie-1.5.7-16.el9.x86_64                      cronie-anacron-1.5.7-16.el9.x86_64        
  crontabs-1.11-26.20190603git.el9.noarch        

Complete!
[root@stapp03 ~]# systemctl start crond
[root@stapp03 ~]# systemctl enable crond
[root@stapp03 ~]# systemctl status crond
● crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-07-15 05:27:11 UTC; 17s ago
   Main PID: 17103 (crond)
      Tasks: 1 (limit: 404712)
     Memory: 952.0K
        CPU: 2ms
     CGroup: /system.slice/crond.service
             └─17103 /usr/sbin/crond -n

Jul 15 05:27:11 stapp03 systemd[1]: Started Command Scheduler.
Jul 15 05:27:11 stapp03 crond[17103]: (CRON) STARTUP (1.5.7)
Jul 15 05:27:11 stapp03 crond[17103]: (CRON) INFO (Syslog will be used instead of sendmail.)
Jul 15 05:27:11 stapp03 crond[17103]: (CRON) INFO (RANDOM_DELAY will be scaled with factor 7>
Jul 15 05:27:11 stapp03 crond[17103]: (CRON) INFO (running with inotify support)
[root@stapp03 ~]# crontab -e
no crontab for root - using an empty one
crontab: installing new crontab
[root@stapp03 ~]# crontab -l
*/5 * * * * echo hello > /tmp/cron_text
[root@stapp03 ~]# ll /tmp/
-bash: ll: command not found
[root@stapp03 ~]# ls -larth /tmp
total 40K
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .font-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .XIM-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .X11-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .ICE-unix
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-rtkit-daemon.service-VDLT0v
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-systemd-logind.service-Sf43Lp
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-upower.service-FZg9Gr
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-dbus-broker.service-7LlGwz
drwxrwxrwt 10 root root 4.0K Jul 15 05:27 .
dr-xr-xr-x  1 root root 4.0K Jul 15 05:29 ..
[root@stapp03 ~]# ls -larth /tmp
total 44K
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .font-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .XIM-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .X11-unix
drwxrwxrwt  2 root root 4.0K Jul 15 05:09 .ICE-unix
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-rtkit-daemon.service-VDLT0v
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-systemd-logind.service-Sf43Lp
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-upower.service-FZg9Gr
drwx------  3 root root 4.0K Jul 15 05:09 systemd-private-f2e7877f98c145bb848aa23b34984b68-dbus-broker.service-7LlGwz
-rw-r--r--  1 root root    6 Jul 15 05:30 cron_text
drwxrwxrwt 10 root root 4.0K Jul 15 05:30 .
dr-xr-xr-x  1 root root 4.0K Jul 15 05:30 ..
```

>CONGRATULATIONS!!!!
You have successfully completed the challenge.Results have been saved. Ref ID:68066aa65500bdf7ab9a7b23
That might have been an easy task for you. But don't worry. Depending on the tasks you complete, you will get more difficult and advanced tasks assigned in the future.
You can also view your results in your dashboard under the "Active Practice" page.