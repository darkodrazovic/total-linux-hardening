Total Linux Hardening
=========

Very restrictive ansible role for the Linux hardening enterprise systems by many standards, so please don't use it on already running machines. The role was planned to be used on newly created systems which will be in use after this hardening, not before.

Role is tested and supported on:
- CentOS 7, 8, 9
- Red Hat 7, 8, 9
- Oracle Linux 7, 8, 9
- SLES_SAP (SLES) 12, 
- SLES 15

The role is created with some recommendations from CIS, Red Hat, Lynis, and others. Testing is done only on minimal server installations, so please use the role with caution and always have a system backup when you run it. Please, do not run the role on production servers, there are much of work with software packages, firewall setup, file system, partition changes, and other sensitive parts of systems and there will be reboots - so, run only after testing on testing servers (like you already do, I hope :-)
Supported file systems for partition hardening: ext3, ext4, xfs.

Also, there is much work for manual steps, like required partitions are missing - you will need to manually fix that or reinstall the machine and start from scratch, so please check the Ansible output.

What it will do
------------
- Full system update and reboot if it is required after update
- Set up motd, issue and issue.net banners
- Set right permission for the /root folder
- Disable NFS services in case /etc/export is empty
- Disable SMB/NMB services
- Harden Diffie-Hellmann moduli
- Install, download and enable sysstat
- Disable USB mass storage
- Disable Firewire ohci driver
- Set Postfix banner
- Disable core dumps
- Set right permission on critical files
- Install Rootkit Hunter (rkhunter)
- Set password expiration the a users to 180 days
- Set nodev, nosuid and noexec options on /tmp, /var, /var/tmp, /var/log, /var/log/audit, /boot partitions (for xfs and ext3/4)
- Set nodev and nosuid options on /home partition (for xfs and ext3/4)
- Set rw, nosuid, nodev, seclabel and noexec options on /dev/shm
- Set defaults, noexec, nosuid options on /dev
- Set umask 027 in /etc/profile, /etc/init.d/functions, /etc/bashrc, /etc/csh.cshrc
- Remove user permission to see pid from other users (set hidepid=2 on /proc)
- Set session timeout to 900 seconds
- Set strict IP for the server in /etc/hosts
- Report to user world writeable paths in in environment variables
- Uninstall unnecessary packages if exists, like x11vnc, rdesktop, skype, etc.
- Set correct permission on log files and set crontab to do that on daily basis
- Disable and remove bash storage (bash history will be removed)
- Set Defaults env_reset in /etc/sudoers if it is not already configured
- Report to user unauthorized accounts with UID=0
- Report to user accounts without password
- Set right permissions on users home folder
- Uninstall insecure packages if exists, like smb, llvm, xinetd...
- Report to user if there is too many users with sudo access
- Set inactive period to 30 days in file /etc/default/useradd
- Lock default services accounts
- Harden all values in /etc/login.defs
- Set correct permission for /usr/bin/as if exists
- Full kernel hardening
- Full SSH hardening
- Change SSH port to 2222
- Disable insecure filesystems sockets protocols like cramfs, squashfs, freevxfs...
- Enable process accounting
- Install yum-cron or dnf-automatic (if applicable)
- Set audit rules
- Install AIDE (if applicable)
- Install OSSEC (if applicable)
- Configure password hashing rounds
- Enable DNSSEC support (if applicable)
- Harden /etc/default/passwd with crypt values (SLES12 only)

To do and what is missing:
- Ensure that firewall is running
- Ensure that selinux is enabled
- Set grub password

Requirements
------------

Ansible server:
- Ansible 2.7+

Target machine(s):
- SSH connection with the sudo access.
- SUSE and RHEL: Python (SUSE minimal doesn't have it)
- RHEL: libselinux-python (for SELinux config on RHEL)
- SUSE: python-xml (for the zypper Ansible plugin)
   - SLES15: SUSEConnect --product sle-module-python2/15.2/x86_64 && zypper in -y python-xml
- Machine connected to a package repository source

Role Variables
--------------

File: defaults/main.yml
  - Setup SSH port (default: 2222)

There are some hardcoded files, like versions of OSSEC, Lynis, rkhunter - please check the content of /files.

Example Playbook
----------------

Create and run playbook:

    - hosts: all
      become: yes
      become_user: root
      become_method: sudo
      gather_facts: true
      ignore_errors: no
      
      roles:
        - total-linux-hardening


Author Information
------------------

Darko Drazovic \
LinkedIn: https://www.linkedin.com/in/darkodrazovic \
Personal: https://darkodrazovic.in.rs \
Blog: https://kompjuteras.com \
GitHub: https://github.com/darkodrazovic
