# Day 4: Script Execution Permissions

In a bid to automate backup processes, the `xFusionCorp Industries` sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on `App Server 1` within the Stratos Datacenter.

Your task is to grant executable permissions to the `/tmp/xfusioncorp.sh` script on `App Server 1`. Additionally, ensure that all users have the capability to execute it.


```bash
thor@jump-host ~$ ssh stapp01 -ltony
The authenticity of host 'stapp01 (10.244.234.213)' can't be established.
ED25519 key fingerprint is SHA256:DuhdALHMFksIDGdq/iJqCLXsya9XsWYC/5NXjNL7oJI.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01' (ED25519) to the list of known hosts.
tony@stapp01's password: 
[tony@stapp01 ~]$ ls -la /tmp/xfusioncorp.sh 
---------- 1 root root 40 Jul 13 05:39 /tmp/xfusioncorp.sh
[tony@stapp01 ~]$ sudo chmod +x+r /tmp/xfusioncorp.sh 

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 

Sorry, try again.
[sudo] password for tony: 
Sorry, try again.
[sudo] password for tony: 
[tony@stapp01 ~]$ ls -la /tmp/xfusioncorp.sh 
-r-xr-xr-x 1 root root 40 Jul 13 05:39 /tmp/xfusioncorp.sh
```

> CONGRATULATIONS!!!!
You have successfully completed the challenge.Results have been saved. Ref ID:6806683d5500bdf7ab9a7b1e
That might have been an easy task for you. But don't worry. Depending on the tasks you complete, you will get more difficult and advanced tasks assigned in the future.
You can also view your results in your dashboard under the "Active Practice" page.