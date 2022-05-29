[![CI](https://github.com/de-it-krachten/ansible-role-rspamd/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-rspamd/actions?query=workflow%3ACI)


# ansible-role-rspamd

Installs & manages rspamd 


Platforms
--------------

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- CentOS 7
- RockyLinux 8
- AlmaLinux 8<sup>1</sup>
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

Role Variables
--------------
<pre><code>
# location of dkim config files
rspamd_dkim_path: /var/lib/rspamd/dkim

# controller password (create using 'rspamadm pw')
rspamd_controller_password: ''

# List of domains
rspamd_domains: []

# Should we use redis
rspamd_use_redis: false

# Postfix setting for spamd integration
rspamd_postfix:
  smtpd_milters:                    'inet:localhost:11332'
  milter_default_action:            accept
  milter_protocol:                  '6'
</pre></code>


Example Playbook
----------------

<pre><code>
- name: sample playbook for role 'rspamd'
  hosts: all
  vars:
    rspamd_controller_password: $2$hrr3pjpiie499r1e7tb1p4qxm84mqeo9$rkgidupktocmsiog5wnm6z93ui9t8jrqpw8ta4sq8dty6djo5bdb
    rspamd_domains: ['example.com', 'foo.bar']
  tasks:
    - name: Include role 'rspamd'
      include_role:
        name: rspamd
</pre></code>
