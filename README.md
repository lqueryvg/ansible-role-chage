# Ansible Module: chage

Query & manage the shadow password file on Linux.

## Features
- provides an interface to the `chage` command
- returns a user's shadow file settings as a dictionary
- python module packaged in a role

## Examples
```yaml
# force password change on next login
- chage: user=john sp_lstchg=0

# remove an account expiration date.
- chage: user=john sp_expire=-1

# set inactivity days after password expired before account is locked
- chage: user=john sp_inact=14

# set both min and max days in single task
- chage: user=john sp_min=7 sp_max=28

# display user password warn days
- chage: user=john
  register: shadow_data
- debug: msg={{data.sp_expire}}             # TODO
```

# Options

Most of the option names follow the fields documented
in `/usr/include/shadow.h` and `pydoc spwd`.

See also `man chage`.

| parameter | required | default | comments
|:----------|:---------|:--------|:-------------|
| user      | yes      |         | user name
| sp_lstchg | no       | None    | chage -d, --lastday    <br>date of last password change 
| sp_min    | no       | None    | chage -m, --mindays    <br>minimum number of days between changes 
| sp_max    | no       | None    | chage -M, --maxdays    <br>maximum number of days between changes 
| sp_warn   | no       | None    | chage -W, --warndays   <br>number of days to warn user to change the password 
| sp_inact  | no       | None    | chage -I, --inactive   <br>number of days the account may be inactive
| sp_expire | no       | None    | chage -E, --expiredate <br>number of days since 1970-01-01 until account expires 

# Requirements

- Python spwd module (included in Python since v2.5 - 2006)
- chage command

# Role Variables

None

# Dependencies

None

# License
GPLv3

# Author Information
[John Buxton](https://github.com/lqueryvg)
