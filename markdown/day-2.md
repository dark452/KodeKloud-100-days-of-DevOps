# Day 2

As part of the temporary assignment to the `Nautilus` project, a developer named `yousuf` requires access for a limited duration. To ensure smooth access management, a temporary user account with an expiry date is needed. Here's what you need to do:

Create a user named `yousuf` on `App Server 1` in Stratos Datacenter. Set the expiry date to `2027-03-28`, ensuring the user is created in lowercase as per standard protocol.

> Note: You can find the infrastructure details by clicking on the Details of all Users and Servers button on the top-right section of the page.

```bash
thor@jump-host ~$ ssh stapp01 -ltony
The authenticity of host 'stapp01 (10.244.81.62)' can't be established.
ED25519 key fingerprint is SHA256:U9RCWbGkySBBwJY+n/uGbg6YZY67We9kG250KL89wxA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp01' (ED25519) to the list of known hosts.
tony@stapp01's password: 

[tony@stapp01 ~]$ sudo adduser yousuf -e 2027-03-28

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for tony: 
```

To check:

```bash
[tony@stapp01 ~]$ sudo chage -l yousuf
Last password change                                    : Jun 30, 2026
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Mar 28, 2027
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```

> CONGRATULATIONS!!!!
You have successfully completed the challenge.Results have been saved. Ref ID:680218275500bdf7ab9a7afa
That might have been an easy task for you. But don't worry. Depending on the tasks you complete, you will get more difficult and advanced tasks assigned in the future.
You can also view your results in your dashboard under the "Active Practice" page