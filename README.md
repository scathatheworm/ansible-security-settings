[![Build Status](https://travis-ci.org/scathatheworm/ansible-security-settings.svg)](https://travis-ci.org/scathatheworm/ansible-security-settings) [![Ansible Galaxy](http://img.shields.io/badge/galaxy-scathatheworm.security-settings.svg)](https://galaxy.ansible.com/scathatheworm/security-settings)

# ansible-security-settings
Ansible Role for enforcing security settings related to enterprise compliance on enterprise grade OS

## Description

This role configures several security settings across login, password management, ssh, pam, selinux configuration and other. It is designed for enterprise compliance definitions.

Configures:

* pam tally and faillock modules for automated account lockout on login failures
* password history
* password complexity
* ssh port, rootlogin, banner, cipher, port forwarding settings
* selinux and firewall state
* shell timeout
* physical sendbreak and ctrl-alt-del disabling
* Linux auditd configuration
* firewall status
* Magic SysRq configuration


# Password aging variables:

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `os_auth_pw_max_age` | 60 | Max days a password is valid before requiring a change |
| `os_auth_pw_min_age` | 10 | Min days of age a password must have before it can be changed |
| `os_auth_pw_warn_age` | 7 | Days before password expires that account will be warned |
| `passhistory` | 6 | number of passwords to remember to avoid reusage |


# Password complexity variables:

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `pwquality_minlen` | 8 | Minimum password length in characters |
| `pwquality_maxrepeat` | 3 | Maximum amount of same characters repeated in password |
| `pwquality_lcredit` | -1 | lowercase amount of chars that must be present in password, for 2 use '-2' and so on |
| `pwquality_ucredit` | -1 | uppercase amount of chars that must be present in password , for 2 use '-2' and so on |
| `pwquality_dcredit` | -1 | digits that must be present in password, for 2 use '-2' and so on |
| `pwquality_ocredit` | -1 | special chars that must be present in a password, for 2 use '-2' and so on |
| `solaris_dictionary_minwordlength` | 5 | Solaris minimum dictionary word length |


# Account inactivity and failed login variables:

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fail_deny` | 5 | Amount of times failed passwords can be tried before locking the account |
| `fail_unlock` | 0 | Seconds elapsed before account is unlocked after failed logins, if set to 0 auto-unlock is disabled and passwords will remain locked |
| `inactive_lock` | 0 | Number of days an account can be inactive before it is locked, a value of 0 disables inactivity lockout |
| `shell_timeout` | 900 | desired shell timeout in seconds, set 0 to disable |


# System services and settings variables:

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `selinux_state` | permissive | selinux configuration value |
| `firewall_check` | false | Configures if the role should check firewall setup |
| `firewall_state` | stopped | Firewall desired status |
| `firewall_enable` |'no' | Desired firewall configuration status |
| `disable_ctrlaltdel` | True | Whether to disable Control-Alt-Del and physical sendbreak in Solaris |
| `solaris_disable_services` | false | Disable unsafe solaris services |
| `magic_sysrq` | 1 | Value of kernel.sysrq setting in Linux, as accepted by the Linux kernel |


# SSH configuration variables:

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `sshrootlogin` | 'no' | allow ssh root login, keep single quotes to avoid boolean evaluation |
| `sshportforwarding` | 'no' | Configured options for port forwarding, values as in config file: yes, no, remote, local |
| `sshmainport` | 22 | main ssh port |
| `sshextraport` | 0 | secondary ssh port, set to 0 to disable an extra port |
| `setloginbanner` | true | use a login banner in ssh |
| `sshd_solaris_restrict_ipv4` | True | Restrict ssh connections to ipv4 in solaris as workaround for DISPLAY issues |
| `ssh_enforce_ciphers` | True | Enforce strong ciphers and MACs in ssh, false to disable and allow all supported MACs and Ciphers |
| `sha1_mac_enabled` | False | Disable use of sha1 HMACs in ssh, theoretical attack vectors exist |
| `md5_mac_enabled` | False | Disable use of md5 HMACs in ssh, known vulnerabilies and attack vectors exist |
| `truncated_mac_enabled` | False | Disable use of md5 or sha1 truncated 96bit HMACs in ssh, shorter subsets of md5 and sha1 |
| `cbc_ciphers_enabled` | False | Disable use of Cipher Block Chaining mode ciphers in ssh, considered vulnerable to several Padding on Oracle attacks |
| `sweet32_ciphers_enabled` | False | Enable use of 64bit Cipher Block Chaining mode ciphers in ssh, considered vulnerable to SWEET32 attack |
| `rc4_ciphers_enabled` | False | Enable use of arcfour ciphers in ssh, considered vulnerable with practical attacks in existance |
| `nist_curves_enabled` | false | Disable NIST KEX curve cryptography since they are weak in several aspects |
| `logjam_sha1_enabled` | false | Disable SHA1 KEX algorithms, vulnerable to logjam attacks |


# Audit configuration variables:

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `auditd_configure:` | true | Enable auditd configuration management |
| `auditd_max_log_filesize` | 25 | Audit max log filesize in MB |
| `auditd_num_logs` | 8 | Max number of audit logs to keep |
| `security_audit_datetime_changes` | true | auditd will track all date or time modifications |
| `security_audit_account_modifications` | true | auditd will track all account modifications |
| `security_audit_network_changes` | true | auditd will track all network config modifications |
| `security_audit_selinux_changes` | true | auditd will track changes to selinux configurations |
| `security_audit_permission_changes` | false | auditd will track all file permission changes |
| `security_audit_fileaccess_failedattempts` | false | auditd will track all unauthorized access attempts to files |
| `security_audit_filesystem_mounts` | true | auditd will track all mount/unmount of filesystems }
| `security_audit_deletions` | false | auditd will track all file deletions |
| `security_audit_sudoers` | true | auditd will track all modification to sudoers rules |
| `security_audit_kernel_modules` | false | auditd will track all module operations as well as sysctl configurations |
| `security_audit_logon` | true | auditd will track all login/logout/sessions |
| `security_audit_elevated_privilege_commands` | true | auditd will track all elevated privilege commands |
| `security_audit_all_commands` | false | auditd will track ALL commands |
| `security_audit_log_integrity` | false | auditd will monitor integrity of logs and logging configuration |
| `security_audit_configuration_immutable` | false | auditd will make its rules immutable, a reboot will be needed to make changes |
| `security_audit_custom_rules` | empty | This is a multi-line variable, that if defined, should contain complete audit rules that will get added to the configuration file |
