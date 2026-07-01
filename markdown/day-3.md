# Day 3: Secure Root SSH Access

Following security audits, the `xFusionCorp Industries` security team has rolled out new protocols, including the restriction of direct root SSH login.

Your task is to disable direct SSH root login on all app servers within the `Stratos Datacenter`.

## Answer

Go to each app server (`stapp01`, `stapp02` and `stapp03`)

edit `/etc/ssh/sshd_config`

find `PermitRootLogin yes` and update it to `PermitRootLogin no`

the restart the service:

```bash
sudo systemctl restart sshd
```

Example:

```bash
[root@stapp03 banner]# vi /etc/ssh/sshd_config
[root@stapp03 banner]# systemctl restart sshd
[root@stapp03 banner]# systemctl status sshd
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.se>
     Active: active (running) since Wed 2026-07-01 0>
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 14959 (sshd)
      Tasks: 1 (limit: 404712)
     Memory: 1.4M
        CPU: 7ms
     CGroup: /system.slice/sshd.service
             └─14959 "sshd: /usr/sbin/sshd -D [liste>

Jul 01 02:06:22 stapp03 systemd[1]: Starting OpenSSH>
Jul 01 02:06:22 stapp03 sshd[14959]: Server listenin>
Jul 01 02:06:22 stapp03 sshd[14959]: Server listenin>
Jul 01 02:06:22 stapp03 systemd[1]: Started OpenSSH 
```

>CONGRATULATIONS!!!!
You have successfully completed the challenge.Results have been saved. Ref ID:68021aa35500bdf7ab9a7afc
That might have been an easy task for you. But don't worry. Depending on the tasks you complete, you will get more difficult and advanced tasks assigned in the future.
You can also view your results in your dashboard under the "Active Practice" page.