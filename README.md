Total Linux Hardening
=========

Very restrictive ansible role for the Linux hardening enterprise systems by many standards (CIS/Lynis/Red Hat/etc.), so please don't use it on already running machines. The role was planned to be used on newly created systems which will be in use after this hardening, not before.

Currently, supported systems are:
- CentOS 7, 8, 9
- Red Hat 7, 8, 9
- SLES_SAP (SLES) 12, 15

The role is created with some recommendations from CIS, Red Hat, Lynis, and my own. Testing is done only on minimal server installations, so please use the role with caution and always have a system backup when you run it. Please, do not run the role on production servers, there are much of work with software packages, firewall setup, file system, partition changes, and other sensitive parts of systems and there will be reboots - so, run only after testing on testing servers (like you already do, I hope :-)
Supported file systems for partition hardening: ext3, ext4, xfs.

Some dangerous tasks from the role are worth mentioning:
- Kernel hardening
- GRUB changes (like the setup of single-user password)
- File system changes for many partitions (like /home, /proc, /boot, etc.) with the new mount options
- Disabling many file systems, like fat32, vfat...
- Firewall will be configured to allow only ping and ssh
- Forget simple passwords, and root will not be able to log in anymore
- New umask values
- Permissions will be changed on many sensitive files and folders
- Many system users will be removed from the system, or their shells will be changed
- Removal of httpd, NFS, dovecot, NTP, smb, xinetd, vsftpd, FTP, etc...
- Many services will be disabled, like cups, smb, xinetd, anacron, and NFS...
- Removal and disable of bash history
- Setup of session timeout
- Regular users will not be allowed to see processes other than own 
- SSH hardening (and by default, SSH port will be changed to 2222)
- Banners will be configured, and old ones will be removed

Also, there is much work for manual steps, like required partitions are missing - you will need to manually fix that or reinstall the machine and start from scratch, so please check the Ansible output.

What is missing
=========
- AppArmor setup for SLES setup
- Test on SLE 12 and SLE 15 (I am sure it will work as usual, but I didn't test it. I did tests on SLES for SAP Applications)
- OSSEC setup
- Debian hardening
- Ubuntu Server hardening


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
  - Setup rsyslog port (default: 514)
  - Setup SSH port (default: 2222)
  - Disable bash history? (default: true)
  - Setup session timeout (default: 900)
  - Do the system needs to use GUI (default: false)
  - Is the system configured as a web server, FTP, or mail server (default: false)
  - Chose do you want to install AIDE (default: true)
  - Server can be rebooted? (default: true)
  - selinux: "enforcing"         
  - selinux_policy: "targeted"

There are some hardcoded files, like versions of OSSEC, Lynis, rkhunter - please check the content of /files.

Dependencies
------------

Used some tasks and files from cis-security role: https://galaxy.ansible.com/dsglaser/cis_security but not dependent.

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
