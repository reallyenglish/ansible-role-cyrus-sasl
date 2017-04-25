# ansible-role-cyrus-sasl

Installs `cyrus-sasl`, `saslauthd` and configures users in `sasl2.db`. This
role supports local SASL database only. If you want other back-ends, this is
not for you.

## `sasl_check_pw`

This role installs a python script to check password of a user under
`/usr/local/bin` on all platforms. As there is no way to see if the password
defined in a role variable and the one in the database are same, the script
reads `cyrus_sasl_sasldb_file` and looks for the user and its password. The
script assumes that `cyrus_sasl_sasldb_file` is `dbm` format.

```sh
sasl_check_pw path user domain
```

`path` is the path to `cyrus_sasl_sasldb_file`. `user` is the user name in
`cyrus_sasl_sasldb_file`, and `domain` is the domain name where the user belong
to. Set the password to check in an environment variable, `userPassword`. When
the password matches the one in `cyrus_sasl_sasldb_file`, it prints `matched`
to `stdout`. When it does not, it prints `does not match` to `stdout`. It must
be executed by a user with read permission on `cyrus_sasl_sasldb_file`.

# Requirements

None

# Role Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `cyrus_sasl_user` | a dict of SASL users to manage (see below) | `{}` |
| `cyrus_sasl_package` | the package name of `cyrus-sasl` | `{{ __cyrus_sasl_package }}` |
| `cyrus_sasl_saslauthd_service` | the service name of `saslauthd` | `{{ __cyrus_sasl_saslauthd_service }}` |
| `cyrus_sasl_saslpassword_command` | the command to manage password of users | `{{ __cyrus_sasl_saslpassword_command }}` |
| `cyrus_sasl_sasldblistusers_command` | the command to list users in the database | `{{ __cyrus_sasl_sasldblistusers_command }}` |
| `cyrus_sasl_sasldb_file` | path to `sasl2.db` | `{{ __cyrus_sasl_sasldb_file }}` |

## `cyrus_sasl_user`

The key is user name and its values is a dict.

| Key | Value |
|-----|-------|
| `domain` | the domain of the user |
| `password` | the password of the user |
| `appname` | the `appname` of the user |
| `state` | either `present` or `absent`. The role creates the user if `present`, or deletes if `absent` |

```
cyrus_sasl_user:
  foo:
    domain: reallyenglish.com
    password: password
    appname: argus
    state: present
```
## Debian

| Variable | Default |
|----------|---------|
| `__cyrus_sasl_package` | `libsasl2-2` |
| `__cyrus_sasl_saslauthd_service` | `saslauthd` |
| `__cyrus_sasl_saslpassword_command` | `saslpasswd2` |
| `__cyrus_sasl_sasldblistusers_command` | `sasldblistusers2` |
| `__cyrus_sasl_sasldb_file` | `/etc/sasldb2` |

## FreeBSD

| Variable | Default |
|----------|---------|
| `__cyrus_sasl_package` | `cyrus-sasl` |
| `__cyrus_sasl_saslauthd_service` | `saslauthd` |
| `__cyrus_sasl_saslpassword_command` | `saslpasswd2` |
| `__cyrus_sasl_sasldblistusers_command` | `sasldblistusers2` |
| `__cyrus_sasl_sasldb_file` | `/usr/local/etc/sasldb2` |

## OpenBSD

| Variable | Default |
|----------|---------|
| `__cyrus_sasl_package` | `cyrus-sasl--` |
| `__cyrus_sasl_saslauthd_service` | `saslauthd` |
| `__cyrus_sasl_saslpassword_command` | `saslpasswd2` |
| `__cyrus_sasl_sasldblistusers_command` | `sasldblistusers2` |
| `__cyrus_sasl_sasldb_file` | `/etc/sasldb2` |

## RedHat

| Variable | Default |
|----------|---------|
| `__cyrus_sasl_package` | `cyrus-sasl` |
| `__cyrus_sasl_saslauthd_service` | `saslauthd` |
| `__cyrus_sasl_saslpassword_command` | `saslpasswd2` |
| `__cyrus_sasl_sasldblistusers_command` | `sasldblistusers2` |
| `__cyrus_sasl_sasldb_file` | `/etc/sasldb2` |

# Dependencies

None

# Example Playbook

```yaml
- hosts: localhost
  roles:
    - ansible-role-cyrus-sasl
  vars:
    cyrus_sasl_user:
      foo:
        domain: reallyenglish.com
        password: password
        appname: argus
        state: present
```

# License

```
Copyright (c) 2017 Tomoyuki Sakurai <tomoyukis@reallyenglish.com>

Permission to use, copy, modify, and distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
```

# Author Information

Tomoyuki Sakurai <tomoyukis@reallyenglish.com>

This README was created by [qansible](https://github.com/trombik/qansible)
