# Ansible Module: chage

[![Build Status](https://travis-ci.org/lqueryvg/ansible-role-chage.svg?branch=master)](https://travis-ci.org/lqueryvg/ansible-role-chage)

Query & manage the shadow password file on Linux.

Ansible Galaxy Page: https://galaxy.ansible.com/lqueryvg/chage/

Please star this repo if you think it's useful.

With the right encouragement I will try to get it added as a standard Ansible module.

## Features
- provides an interface to the `chage` command
- returns a user's shadow file settings as a dictionary
- python module packaged in a role

## Examples
```yaml
  # force password change on next login
  - chage: user=john sp_lstchg=0

  # or using argument alias:
  - chage: user=john lastday=0

  # remove an account expiration date.
  - chage: user=john sp_expire=-1

  # set inactivity days after password expired before account is locked
  - chage: user=john sp_inact=14

  # set both min and max days in single task
  - chage: user=john sp_min=7 sp_max=28

  # retrieve then access user's password expiry days
  - chage: user=john
    register: result

  - debug: msg={{result.shadow.sp_expire}}
```

# Options

Most of the option names follow the fields documented
in `/usr/include/shadow.h` and `pydoc spwd`.

See also `man chage`.

| argument  | alias      | required | default | comments
|:----------|:-----------|:---------|:--------|:-------------|
| user      |            | yes      |         | user name
| sp_lstchg | lastday    | no       | None    | chage -d, --lastday    <br>days since 1970/01/01 when password was last changed or date in format YYYY-MM-DD
| sp_min    | mindays    | no       | None    | chage -m, --mindays    <br>minimum number of days between changes 
| sp_max    | maxdays    | no       | None    | chage -M, --maxdays    <br>maximum number of days between changes or remove with -1
| sp_warn   | warndays   | no       | None    | chage -W, --warndays   <br>number of days before password expiry to warn user to change password
| sp_inact  | inactive   | no       | None    | chage -I, --inactive   <br>set number of days the account may be inactive remove the field by passing value of -1
| sp_expire | expiredate | no       | None    | chage -E, --expiredate <br>days since 1970-01-01 until account expires or date in format YYYY-MM-DD$

Note: argument aliases (e.g. `lastday`) are to assist those who are already
familiar with the `chage` command flags, however I would recommend using the
actual argument names (e.g. `sp_lstchg`) instead for the following reasons:

1. The data structure fields names returned by `chage` will match the real
   argument names, even if you used alias names in the playbook task.
2. Libraries (e.g. Python `spwd` module) are more likely to use the real
   argument names which in turn match the `/usr/include/shadow.h` and the C
   library which uses it. Although this Ansible module is called `chage`,
   it's real intention is to provide an interface to the `/etc/shadow` file.

# Requirements

- `chage` command
- `/etc/shadow` file (read pwconv man page if `/etc/shadow` does not exist)
- root access (to read `/etc/shadow` file)

# Role Variables

None

# Dependencies

None

# License
GPLv3

# Author Information
[John Buxton](https://github.com/lqueryvg)
