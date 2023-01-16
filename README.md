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
