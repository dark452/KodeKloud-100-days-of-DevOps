# Day 5: SElinux Installation and Configuration

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for `App server 3` in the `Stratos Datacenter`:

1. Install the required `SELinux` packages.
2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
4. Disregard the current status of SELinux via the command line; the final status after the reboot should be `disabled`.


```bash
thor@jump-host ~$ ssh stapp03 -lbanner
The authenticity of host 'stapp03 (10.244.81.9)' can't be established.
ED25519 key fingerprint is SHA256:xiBM6wqJXlVp1/EO3nudrJwl8TK/nEj4yaBZj9Zdojk.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'stapp03' (ED25519) to the list of known hosts.
banner@stapp03's password: 
[banner@stapp03 ~]$ sudo dnf search selinux

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for banner: 
CentOS Stream 9 - BaseOS                      6.4 MB/s | 9.0 MB     00:01    
CentOS Stream 9 - AppStream                    35 MB/s |  28 MB     00:00    
CentOS Stream 9 - Extras packages              59 kB/s |  21 kB     00:00    
Extra Packages for Enterprise Linux 9 - x86_6  21 MB/s |  21 MB     00:00    
Extra Packages for Enterprise Linux 9 openh26 2.8 kB/s | 2.5 kB     00:00    
Extra Packages for Enterprise Linux 9 - Next  1.3 MB/s | 265 kB     00:00    
====================== Name & Summary Matched: selinux =======================
adcli-selinux.noarch : The adcli SELinux policy
bitcoin-core-selinux.noarch : Bitcoin Core SELinux policy
bluechi-selinux.noarch : BlueChi SELinux policy
cepces-selinux.noarch : SELinux support for cepces
cjdns-selinux.noarch : Targeted SELinux policy module for cjdns
cobbler-selinux.noarch : SELinux policies for cobbler
cockpit-ws-selinux.x86_64 : SELinux security policy for cockpit-ws
container-selinux.noarch : SELinux policies for container runtimes
copr-selinux.noarch : SELinux module for COPR
dist-git-selinux.noarch : SELinux support for dist-git
dnsconfd-selinux.noarch : dnsconfd SELinux policy
drbd-selinux.x86_64 : SElinux policy for DRBD
duo_unix-selinux.x86_64 : SELinux rules for duo_unix
ec2-instance-connect-selinux.noarch : ec2-instance-connect SELinux policy
efs-utils-selinux.noarch : efs-utils SELinux policy
fail2ban-selinux.noarch : SELinux policies for Fail2Ban
fapolicyd-selinux.noarch : Fapolicyd selinux
flatpak-selinux.noarch : SELinux policy module for flatpak
frr-selinux.noarch : Selinux policy for FRR
frr10-selinux.noarch : Selinux policy for FRR
grafana-selinux.x86_64 : SELinux policy module supporting grafana
hirte-selinux.x86_64 : Hirte SELinux policy
insights-core-selinux.noarch : Insights Core SELinux policy
ipa-selinux.noarch : FreeIPA SELinux policy
ipa-selinux-luna.noarch : FreeIPA SELinux policy for Thales Luna HSMs
ipa-selinux-nfast.noarch : FreeIPA SELinux policy for nCipher nfast HSMs
keylime-selinux.noarch : keylime SELinux policy
lemonldap-ng-selinux.noarch : LemonLDAP-NG SELinux policy
libselinux.i686 : SELinux library and simple utilities
libselinux.x86_64 : SELinux library and simple utilities
libselinux-devel.i686 : Header files and libraries used to build SELinux
libselinux-devel.x86_64 : Header files and libraries used to build SELinux
libselinux-ruby.x86_64 : SELinux ruby bindings for libselinux
libselinux-utils.x86_64 : SELinux libselinux utilities
memcached-selinux.x86_64 : Selinux policy module
mlmmj-selinux.noarch : SELinux support for mlmmj
mysql-selinux.noarch : SELinux policy modules for MySQL and MariaDB packages
nagios-selinux.noarch : SELinux context for nagios
nbdkit-selinux.noarch : nbdkit SELinux policy
nrpe-selinux.x86_64 : SELinux context for nrpe
openssh-ldap-authkeys-selinux.noarch : SELinux module for
                                     : openssh-ldap-authkeys
osbuild-container-selinux.x86_64 : SELinux container policies
osbuild-selinux.noarch : SELinux policies
osbuild-selinux.x86_64 : SELinux policies
passt-selinux.noarch : SELinux support for passt and pasta
pcp-selinux.x86_64 : Selinux policy package
php-pecl-selinux.x86_64 : SELinux binding for PHP scripting language
postgresql16-credcheck-selinux.noarch : credcheck SELinux policy
pure-ftpd-selinux.x86_64 : SELinux support for Pure-FTPD
python-pymilter-selinux.noarch : SELinux policy module for pymilter
python3-libselinux.x86_64 : SELinux python 3 bindings for libselinux
radicale3-selinux.noarch : SELinux definitions for Radicale
rd-agent-selinux.noarch : SELinux policy for rd-agent
resalloc-selinux.noarch : SELinux module for resalloc
rpm-plugin-selinux.x86_64 : Rpm plugin for SELinux functionality
selinux-policy.noarch : SELinux policy configuration
selinux-policy-automotive.noarch : SELinux automotive policy
selinux-policy-devel.noarch : SELinux policy development files
selinux-policy-doc.noarch : SELinux policy documentation
selinux-policy-minimum.noarch : SELinux minimum policy
selinux-policy-mls.noarch : SELinux MLS policy
selinux-policy-sandbox.noarch : SELinux sandbox policy
selinux-policy-targeted.noarch : SELinux targeted policy
snapd-selinux.noarch : SELinux module for snapd
tigervnc-selinux.noarch : SELinux module for TigerVNC
tpm2-abrmd-selinux.noarch : SELinux policies for tpm2-abrmd
trafficserver-selinux.noarch : trafficserver SELinux policy
usbguard-selinux.noarch : USBGuard selinux
vsomeip3-selinux.noarch : SELinux policy module for vsomeip3
websvn-selinux.noarch : SELinux context for websvn
xrdp-selinux.x86_64 : SELinux policy module required tu run xrdp
xrootd-selinux.noarch : SELinux policy module for the xrootd server
zabbix-selinux.noarch : Zabbix SELinux policy
zabbix7.0-selinux.noarch : Zabbix SELinux policy
=========================== Name Matched: selinux ============================
rust-coreutils+feat_require_selinux-devel.noarch : coreutils ~ GNU coreutils
                                                 : reimplementation in Rust
rust-coreutils+feat_selinux-devel.noarch : coreutils ~ GNU coreutils
                                         : reimplementation in Rust
rust-coreutils+selinux-devel.noarch : coreutils ~ GNU coreutils
                                    : reimplementation in Rust
rust-selinux+default-devel.noarch : Flexible Mandatory Access Control for
                                  : Linux
rust-selinux-devel.noarch : Flexible Mandatory Access Control for Linux
rust-selinux-sys+default-devel.noarch : Flexible Mandatory Access Control
                                      : (MAC) for Linux
rust-selinux-sys-devel.noarch : Flexible Mandatory Access Control (MAC) for
                              : Linux
rust-selinux0.4+default-devel.noarch : Flexible Mandatory Access Control for
                                     : Linux
rust-selinux0.4-devel.noarch : Flexible Mandatory Access Control for Linux
rust-uu_cp+feat_selinux-devel.noarch : cp ~ (uutils) copy SOURCE to
                                     : DESTINATION
rust-uu_cp+selinux-devel.noarch : cp ~ (uutils) copy SOURCE to DESTINATION
rust-uu_id+feat_selinux-devel.noarch : id ~ (uutils) display user and group
                                     : information for USER
rust-uu_id+selinux-devel.noarch : id ~ (uutils) display user and group
                                : information for USER
rust-uu_install+selinux-devel.noarch : install - (uutils) copy files from
                                     : SOURCE to DESTINATION
rust-uu_ls+feat_selinux-devel.noarch : ls ~ (uutils) display directory
                                     : contents
rust-uu_ls+selinux-devel.noarch : ls ~ (uutils) display directory contents
rust-uu_mkdir+selinux-devel.noarch : mkdir ~ (uutils) create DIRECTORY
rust-uu_mkfifo+selinux-devel.noarch : mkfifo ~ (uutils) create FIFOs (named
                                    : pipes)
rust-uu_mknod+selinux-devel.noarch : mknod ~ (uutils) create special file NAME
                                   : of TYPE
rust-uu_mv+selinux-devel.noarch : mv ~ (uutils) move (rename) SOURCE to
                                : DESTINATION
rust-uu_stat+selinux-devel.noarch : stat ~ (uutils) display FILE status
rust-uucore+selinux-devel.noarch : Uutils ~ 'core' uutils code library
                                 : (cross-platform)
rust-x11rb+xselinux-devel.noarch : Rust bindings to X11
rust-x11rb-protocol+xselinux-devel.noarch : Rust bindings to X11
========================== Summary Matched: selinux ==========================
checkpolicy.x86_64 : SELinux policy compiler
libsemanage.i686 : SELinux binary policy manipulation library
libsemanage.x86_64 : SELinux binary policy manipulation library
libsepol.i686 : SELinux binary policy manipulation library
libsepol.x86_64 : SELinux binary policy manipulation library
libsepol-utils.x86_64 : SELinux libsepol utilities
mcstrans.x86_64 : SELinux Translation Daemon
policycoreutils.x86_64 : SELinux policy core utilities
policycoreutils-dbus.noarch : SELinux policy core DBUS api
policycoreutils-devel.i686 : SELinux policy core policy devel utilities
policycoreutils-devel.x86_64 : SELinux policy core policy devel utilities
policycoreutils-gui.noarch : SELinux configuration GUI
policycoreutils-python-utils.noarch : SELinux policy core python utilities
policycoreutils-restorecond.x86_64 : SELinux restorecond utilities
policycoreutils-sandbox.x86_64 : SELinux sandbox utilities
python3-policycoreutils.noarch : SELinux policy core python3 interfaces
python3-setools.x86_64 : Policy analysis tools for SELinux
secilc.x86_64 : The SELinux CIL Compiler
secilc-doc.noarch : Documentation for the SELinux CIL Compiler
selint.x86_64 : Static code analysis tool for SELinux policy source files
setools.x86_64 : Policy analysis tools for SELinux
setools-console.x86_64 : Policy analysis command-line tools for SELinux
setools-console-analyses.x86_64 : Policy analysis command-line tools for
                                : SELinux
setools-gui.x86_64 : Policy analysis graphical tools for SELinux
setroubleshoot.x86_64 : Helps troubleshoot SELinux problems
setroubleshoot-server.x86_64 : SELinux troubleshoot server
udica.noarch : A tool for generating SELinux security policies for containers
[banner@stapp03 ~]$ sudo dnf list selinux
Last metadata expiration check: 0:00:53 ago on Wed Jul 15 05:05:49 2026.
Error: No matching Packages to list
[banner@stapp03 ~]$ sudo -i
[root@stapp03 ~]# dnf install selinux-policy policycoreutils selinux-target
Last metadata expiration check: 0:03:33 ago on Wed Jul 15 05:05:49 2026.
Package policycoreutils-3.6-2.1.el9.x86_64 is already installed.
No match for argument: selinux-target
Error: Unable to find a match: selinux-target
[root@stapp03 ~]# dnf install selinux-policy policycoreutils selinux-policy-target
Last metadata expiration check: 0:03:49 ago on Wed Jul 15 05:05:49 2026.
Package policycoreutils-3.6-2.1.el9.x86_64 is already installed.
No match for argument: selinux-policy-target
Error: Unable to find a match: selinux-policy-target
[root@stapp03 ~]# dnf install selinux-policy policycoreutils selinux-policy-targeted
Last metadata expiration check: 0:04:03 ago on Wed Jul 15 05:05:49 2026.
Package policycoreutils-3.6-2.1.el9.x86_64 is already installed.
Dependencies resolved.
==============================================================================
 Package                        Arch     Version            Repository   Size
==============================================================================
Installing:
 selinux-policy                 noarch   38.1.83-1.el9      baseos       42 k
 selinux-policy-targeted        noarch   38.1.83-1.el9      baseos      6.9 M
Upgrading:
 audit-libs                     x86_64   3.1.5-8.el9        baseos      121 k
 policycoreutils                x86_64   3.6-7.el9          baseos      238 k
 python3-rpm                    x86_64   4.16.1.3-37.el9    baseos       65 k
 rpm                            x86_64   4.16.1.3-37.el9    baseos      536 k
 rpm-build-libs                 x86_64   4.16.1.3-37.el9    baseos       89 k
 rpm-libs                       x86_64   4.16.1.3-37.el9    baseos      308 k
 rpm-sign-libs                  x86_64   4.16.1.3-37.el9    baseos       21 k
Installing dependencies:
 checkpolicy                    x86_64   3.6-1.el9          baseos      353 k
 flatpak-selinux                noarch   1.12.9-1.el9       appstream    22 k
 policycoreutils-python-utils   noarch   3.6-7.el9          baseos       75 k
 python3-audit                  x86_64   3.1.5-8.el9        baseos       82 k
 python3-distro                 noarch   1.5.0-7.el9        baseos       37 k
 python3-libselinux             x86_64   3.6-1.el9          appstream   188 k
 python3-libsemanage            x86_64   3.6-1.el9          appstream    80 k
 python3-policycoreutils        noarch   3.6-7.el9          baseos      2.1 M
 python3-setools                x86_64   4.4.4-1.el9        baseos      605 k
 python3-setuptools             noarch   53.0.0-15.el9      baseos      936 k
 rpm-plugin-selinux             x86_64   4.16.1.3-37.el9    baseos       17 k

Transaction Summary
==============================================================================
Install  13 Packages
Upgrade   7 Packages

Total download size: 13 M
Is this ok [y/N]: y
Downloading Packages:
(1/20): python3-audit-3.1.5-8.el9.x86_64.rpm  370 kB/s |  82 kB     00:00    
(2/20): policycoreutils-python-utils-3.6-7.el 337 kB/s |  75 kB     00:00    
(3/20): python3-distro-1.5.0-7.el9.noarch.rpm 676 kB/s |  37 kB     00:00    
(4/20): checkpolicy-3.6-1.el9.x86_64.rpm      1.1 MB/s | 353 kB     00:00    
(5/20): python3-setuptools-53.0.0-15.el9.noar 8.4 MB/s | 936 kB     00:00    
(6/20): python3-setools-4.4.4-1.el9.x86_64.rp 3.4 MB/s | 605 kB     00:00    
(7/20): rpm-plugin-selinux-4.16.1.3-37.el9.x8 323 kB/s |  17 kB     00:00    
(8/20): python3-policycoreutils-3.6-7.el9.noa 7.5 MB/s | 2.1 MB     00:00    
(9/20): selinux-policy-38.1.83-1.el9.noarch.r 768 kB/s |  42 kB     00:00    
(10/20): python3-libselinux-3.6-1.el9.x86_64. 757 kB/s | 188 kB     00:00    
(11/20): selinux-policy-targeted-38.1.83-1.el  23 MB/s | 6.9 MB     00:00    
(12/20): audit-libs-3.1.5-8.el9.x86_64.rpm    2.2 MB/s | 121 kB     00:00    
(13/20): python3-libsemanage-3.6-1.el9.x86_64 1.0 MB/s |  80 kB     00:00    
(14/20): policycoreutils-3.6-7.el9.x86_64.rpm 4.3 MB/s | 238 kB     00:00    
(15/20): python3-rpm-4.16.1.3-37.el9.x86_64.r 1.2 MB/s |  65 kB     00:00    
(16/20): flatpak-selinux-1.12.9-1.el9.noarch.  53 kB/s |  22 kB     00:00    
(17/20): rpm-4.16.1.3-37.el9.x86_64.rpm       9.2 MB/s | 536 kB     00:00    
(18/20): rpm-build-libs-4.16.1.3-37.el9.x86_6 1.6 MB/s |  89 kB     00:00    
(19/20): rpm-libs-4.16.1.3-37.el9.x86_64.rpm  5.2 MB/s | 308 kB     00:00    
(20/20): rpm-sign-libs-4.16.1.3-37.el9.x86_64 405 kB/s |  21 kB     00:00    
------------------------------------------------------------------------------
Total                                          10 MB/s |  13 MB     00:01     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Running scriptlet: selinux-policy-targeted-38.1.83-1.el9.noarch         1/1 
  Preparing        :                                                      1/1 
  Upgrading        : audit-libs-3.1.5-8.el9.x86_64                       1/27 
  Upgrading        : rpm-4.16.1.3-37.el9.x86_64                          2/27 
  Upgrading        : rpm-libs-4.16.1.3-37.el9.x86_64                     3/27 
  Upgrading        : policycoreutils-3.6-7.el9.x86_64                    4/27 
  Running scriptlet: policycoreutils-3.6-7.el9.x86_64                    4/27 
  Installing       : selinux-policy-38.1.83-1.el9.noarch                 5/27 
  Running scriptlet: selinux-policy-38.1.83-1.el9.noarch                 5/27 
  Running scriptlet: selinux-policy-targeted-38.1.83-1.el9.noarch        6/27 
  Installing       : selinux-policy-targeted-38.1.83-1.el9.noarch        6/27 
  Running scriptlet: selinux-policy-targeted-38.1.83-1.el9.noarch        6/27 
  Installing       : python3-libselinux-3.6-1.el9.x86_64                 7/27 
  Installing       : python3-setuptools-53.0.0-15.el9.noarch             8/27 
  Installing       : python3-distro-1.5.0-7.el9.noarch                   9/27 
  Installing       : python3-setools-4.4.4-1.el9.x86_64                 10/27 
  Installing       : python3-libsemanage-3.6-1.el9.x86_64               11/27 
  Upgrading        : rpm-build-libs-4.16.1.3-37.el9.x86_64              12/27 
  Upgrading        : rpm-sign-libs-4.16.1.3-37.el9.x86_64               13/27 
  Installing       : python3-audit-3.1.5-8.el9.x86_64                   14/27 
  Installing       : checkpolicy-3.6-1.el9.x86_64                       15/27 
  Installing       : python3-policycoreutils-3.6-7.el9.noarch           16/27 
  Installing       : policycoreutils-python-utils-3.6-7.el9.noarch      17/27 
  Installing       : flatpak-selinux-1.12.9-1.el9.noarch                18/27 
  Running scriptlet: flatpak-selinux-1.12.9-1.el9.noarch                18/27 
  Upgrading        : python3-rpm-4.16.1.3-37.el9.x86_64                 19/27 
  Installing       : rpm-plugin-selinux-4.16.1.3-37.el9.x86_64          20/27 
  Cleanup          : python3-rpm-4.16.1.3-30.el9.x86_64                 21/27 
  Cleanup          : rpm-build-libs-4.16.1.3-30.el9.x86_64              22/27 
  Cleanup          : rpm-sign-libs-4.16.1.3-30.el9.x86_64               23/27 
  Running scriptlet: policycoreutils-3.6-2.1.el9.x86_64                 24/27 
  Cleanup          : policycoreutils-3.6-2.1.el9.x86_64                 24/27 
  Cleanup          : rpm-libs-4.16.1.3-30.el9.x86_64                    25/27 
  Cleanup          : rpm-4.16.1.3-30.el9.x86_64                         26/27 
  Cleanup          : audit-libs-3.1.2-2.el9.x86_64                      27/27 
  Running scriptlet: rpm-4.16.1.3-37.el9.x86_64                         27/27 
  Running scriptlet: selinux-policy-targeted-38.1.83-1.el9.noarch       27/27 
  Running scriptlet: audit-libs-3.1.2-2.el9.x86_64                      27/27 
  Verifying        : checkpolicy-3.6-1.el9.x86_64                        1/27 
  Verifying        : policycoreutils-python-utils-3.6-7.el9.noarch       2/27 
  Verifying        : python3-audit-3.1.5-8.el9.x86_64                    3/27 
  Verifying        : python3-distro-1.5.0-7.el9.noarch                   4/27 
  Verifying        : python3-policycoreutils-3.6-7.el9.noarch            5/27 
  Verifying        : python3-setools-4.4.4-1.el9.x86_64                  6/27 
  Verifying        : python3-setuptools-53.0.0-15.el9.noarch             7/27 
  Verifying        : rpm-plugin-selinux-4.16.1.3-37.el9.x86_64           8/27 
  Verifying        : selinux-policy-38.1.83-1.el9.noarch                 9/27 
  Verifying        : selinux-policy-targeted-38.1.83-1.el9.noarch       10/27 
  Verifying        : flatpak-selinux-1.12.9-1.el9.noarch                11/27 
  Verifying        : python3-libselinux-3.6-1.el9.x86_64                12/27 
  Verifying        : python3-libsemanage-3.6-1.el9.x86_64               13/27 
  Verifying        : audit-libs-3.1.5-8.el9.x86_64                      14/27 
  Verifying        : audit-libs-3.1.2-2.el9.x86_64                      15/27 
  Verifying        : policycoreutils-3.6-7.el9.x86_64                   16/27 
  Verifying        : policycoreutils-3.6-2.1.el9.x86_64                 17/27 
  Verifying        : python3-rpm-4.16.1.3-37.el9.x86_64                 18/27 
  Verifying        : python3-rpm-4.16.1.3-30.el9.x86_64                 19/27 
  Verifying        : rpm-4.16.1.3-37.el9.x86_64                         20/27 
  Verifying        : rpm-4.16.1.3-30.el9.x86_64                         21/27 
  Verifying        : rpm-build-libs-4.16.1.3-37.el9.x86_64              22/27 
  Verifying        : rpm-build-libs-4.16.1.3-30.el9.x86_64              23/27 
  Verifying        : rpm-libs-4.16.1.3-37.el9.x86_64                    24/27 
  Verifying        : rpm-libs-4.16.1.3-30.el9.x86_64                    25/27 
  Verifying        : rpm-sign-libs-4.16.1.3-37.el9.x86_64               26/27 
  Verifying        : rpm-sign-libs-4.16.1.3-30.el9.x86_64               27/27 

Upgraded:
  audit-libs-3.1.5-8.el9.x86_64            policycoreutils-3.6-7.el9.x86_64   
  python3-rpm-4.16.1.3-37.el9.x86_64       rpm-4.16.1.3-37.el9.x86_64         
  rpm-build-libs-4.16.1.3-37.el9.x86_64    rpm-libs-4.16.1.3-37.el9.x86_64    
  rpm-sign-libs-4.16.1.3-37.el9.x86_64    
Installed:
  checkpolicy-3.6-1.el9.x86_64                                                
  flatpak-selinux-1.12.9-1.el9.noarch                                         
  policycoreutils-python-utils-3.6-7.el9.noarch                               
  python3-audit-3.1.5-8.el9.x86_64                                            
  python3-distro-1.5.0-7.el9.noarch                                           
  python3-libselinux-3.6-1.el9.x86_64                                         
  python3-libsemanage-3.6-1.el9.x86_64                                        
  python3-policycoreutils-3.6-7.el9.noarch                                    
  python3-setools-4.4.4-1.el9.x86_64                                          
  python3-setuptools-53.0.0-15.el9.noarch                                     
  rpm-plugin-selinux-4.16.1.3-37.el9.x86_64                                   
  selinux-policy-38.1.83-1.el9.noarch                                         
  selinux-policy-targeted-38.1.83-1.el9.noarch                                

Complete!
[root@stapp03 ~]# setenforce 0
setenforce: SELinux is disabled
[root@stapp03 ~]# sestatus 
SELinux status:                 disabled   
[root@stapp03 ~]# vi /etc/selinux/
config         semanage.conf  targeted/      
[root@stapp03 ~]# vi /etc/selinux/config 
[root@stapp03 ~]# cat /etc/selinux/config 

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
# See also:
# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/using_selinux/changing-selinux-states-and-modes_using-selinux#changing-selinux-modes-at-boot-time_changing-selinux-states-and-modes
#
# NOTE: Up to RHEL 8 release included, SELINUX=disabled would also
# fully disable SELinux during boot. If you need a system with SELinux
# fully disabled instead of SELinux running with no policy loaded, you
# need to pass selinux=0 to the kernel command line. You can use grubby
# to persistently set the bootloader to boot with selinux=0:
#
#    grubby --update-kernel ALL --args selinux=0
#
# To revert back to SELinux enabled:
#
#    grubby --update-kernel ALL --remove-args selinux
#
SELINUX=disabled
# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

>CONGRATULATIONS!!!!
You have successfully completed the challenge.Results have been saved. Ref ID:68066a215500bdf7ab9a7b21
That might have been an easy task for you. But don't worry. Depending on the tasks you complete, you will get more difficult and advanced tasks assigned in the future.
You can also view your results in your dashboard under the "Active Practice" page.
