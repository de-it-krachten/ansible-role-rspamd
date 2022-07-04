[![CI](https://github.com/de-it-krachten/ansible-role-rspamd/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-rspamd/actions?query=workflow%3ACI)


# ansible-role-rspamd

Installs & manages rspamd 


## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- CentOS 7
- RockyLinux 8
- OracleLinux 8
- AlmaLinux 8
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 18.04 LTS
- Ubuntu 20.04 LTS

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Use redis
rspamd_use_redis: true

# Activate DMARC reporting
rspamd_dmarc_active: true

# Activate DKIM signing
rspamd_dkim_active: true

# Activate grey listing
rspamd_greylist_active: true

# location of dkim config files
rspamd_dkim_path: /var/lib/rspamd/dkim

# dkim selector
rspamd_dkim_selector: dkim

# controller password (create using 'rspamadm pw')
rspamd_controller_password: ''

# List of domains
rspamd_domains: []

# Postfix setting for spamd integration
rspamd_postfix:
  smtpd_milters:                    'inet:localhost:11332'
  milter_default_action:            accept
  milter_protocol:                  '6'
</pre></code>

### vars/family-RedHat.yml
<pre><code>
# GPG key for testing package integrity
rspamd_gpgkey_url: https://rspamd.com/rpm-stable/gpg.key

# rspamd_repo_url: https://rspamd.com/rpm-stable/{{ ansible_distribution|lower }}-{{ ansible_distribution_major_version }}/rspamd.repo
rspamd_repo_url: https://rspamd.com/rpm-stable/centos-{{ ansible_distribution_major_version }}/rspamd.repo

# List of packages to install
rspamd_packages:
  - rspamd

# service to start/enable
rspamd_service: rspamd
</pre></code>

### vars/family-Debian.yml
<pre><code>
# GPG key for testing package integrity
rspamd_gpgkey_url: https://rspamd.com/apt-stable/gpg.key

# List of packages to install
rspamd_packages:
  - rspamd

# service to start/enable
rspamd_service: rspamd
</pre></code>



## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'rspamd'
  hosts: all
  vars:
    rspamd_organization: Example Inc
    rspamd_controller_password: $2$hrr3pjpiie499r1e7tb1p4qxm84mqeo9$rkgidupktocmsiog5wnm6z93ui9t8jrqpw8ta4sq8dty6djo5bdb
    rspamd_domains: ['example.com', 'foo.bar']
    dovecot_ssl_key: "{{ openssl_server_key }}"
    dovecot_ssl_chain: "{{ openssl_server_crt }}"
    dovecot_domain: example.com
    postfix_ipv6: False
    postfix_domain: example.com
    postfix_fqdn: host.example.com
    postfix_ssl_key: "{{ openssl_server_key }}"
    postfix_ssl_chain: "{{ openssl_server_crt }}"
  pre_tasks:
    - name: Create 'remote_tmp'
      file:
        path: /root/.ansible/tmp
        state: directory
        mode: "0700"
  roles:
    - cron
    - openssl
    - postfix
  tasks:
    - name: Include role 'rspamd'
      include_role:
        name: rspamd
</pre></code>
