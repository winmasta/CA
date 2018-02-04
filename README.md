Role Name
=========

Ansible role for creating self-signed CA and client private key and certificate. More keys and certificates can be
created after deployment using created CA. Tested on:
  - Ubuntu 14.04 Trusty
  - Ubuntu 16.04 Xenial

Requirements
------------

Installed OS:
 - Ubuntu 14.04 Trusty
 - Ubuntu 16.04 Xenial

Role Variables
--------------

- TLS_DIR: "{{ CONSUL_DIR }}/tls" # Directory for TLS files (certificates and keys)
- CA_DIR: "{{ CONSUL_TLS_DIR }}/CA" # Directory for TLS Certificate Authority files (certificates, keys, CSRs)
- FIRST_CA_SERIAL: 000a # First serial number of certificate
- CERT_SERIAL_FILENAME: serial # Database of signed certificates serial numbers
- CERT_DATABASE_FILENAME: certindex # Database of signed certificates
- CA_KEYTYPE: rsa:2048 # Certificate Authority private key type
- CA_PRIVKEY_NAME: ca_privkey.key # Certificate Authority private key file name
- CA_EXPIRE: 3650 # Period, after which Certificate Authority self signed certificate will expire (days)
- CA_CERT_NAME: ca.cert # Certificate Authority self signed certificate file name
- CA_COUNTRY: US # Certificate Authority self signed certificate subject - Country
- CA_STATE: CA # Certificate Authority self signed certificate subject - State
- CA_LOCALITY: Some City # Certificate Authority self signed certificate subject - Locality
- CA_ORG_NAME: Some Company # Certificate Authority self signed certificate subject - Organization
- CA_ORG_UNIT_NAME: Some dept # Certificate Authority self signed certificate subject - Organization unit name
- CA_COMMON_NAME: Consul Staging Server # Certificate Authority self signed certificate subject - Common name
- CA_EMAIL: dept@company.com # Certificate Authority self signed certificate subject - E-mail
- CA_NAME: af_ca # Certificate Authority config file name (without extension)
- CLIENT_KEYTYPE: rsa:1024 # Client private key type
- CLIENT_CSR_FILENAME: client.csr # Client CSR filename
- CLIENT_KEY_FILENAME: client.key # Client private key filename
- CLIENT_COUNTRY: US # Client private key subject - Country
- CLIENT_STATE: CA # Client certificate subject - State
- CLIENT_LOCALITY: Some City # Client certificate subject - Locality
- CLIENT_ORG_NAME: Some Company # Client certificate subject - Organization
- CLIENT_ORG_UNIT_NAME: Some dept # Client certificate subject - Organization unit name
- CLIENT_COMMON_NAME: "\*.company.com" # Client certificate subject - Common name, can be wildcard
- CLIENT_EMAIL: dept@company.com # Client certificate subject - Email
- CLIENT_CERT_FILENAME: client.cert # Client certificate filename

Example Playbook
----------------

To use this role:

  - create folder (in user $HOME folder in example below) and install role with dependencies from ansible-galaxy

```bash
cd ~/
mkdir CA
cd CA
ansible-galaxy install winmasta.CA --roles-path .
```

  - create file `hosts`, containing hostname(s) or IP address(es) of host(s), where you want to deploy the role

```bash
echo "ENTER HOSTNAME OR IP" > hosts
```

  - create file `ansible.cfg` in current folder

```bash
cat > ansible.cfg << EOF
[defaults]
remote_user = root
host_key_checking = False
EOF
```

  - create playbook in current folder `main.yml` with content

```bash
cat > main.yml << EOF
---
- hosts: all
  gather_facts: no

  pre_tasks:

  - name: Install required packages
    raw: sudo apt-get update -y && sudo apt-get -y install python-simplejson python-pip
    changed_when: False

  - setup:

  roles:
    - winmasta.CA
EOF
```

  - execute playbook `main.yml`

```bash
ansible-playbook -i hosts main.yml
```

License
-------

MIT
