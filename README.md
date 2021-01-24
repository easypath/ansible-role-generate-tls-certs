Generate TLS certificates
=========================
Generates self-signed CA, client and server certificates. Runs locally on control machine.

Notes:
- Will not overwrite any files in output cert dir
- Will not copy the files to the remote servers if the local files are unchanged
- Will optionally (see `gen_tls_populate_etc_hosts` variable) add to each machine's `/etc/hosts`
  a line for each host in the inventory.


Requirements
------------
- For server certificates, must specify Ansible inventory file; FQDN must also be set as hostname in inventory file


Role Variables
--------------
See `defaults/main.yml`


Dependencies
------------

Install dependencies via

```
$ ansible-galaxy collection install community.crypto
```

Example Playbook
----------------

The provided example `playbook.yml` targets two hosts (take a look at the
`Vagrantfile`).

All the cryptographic relevant operations are performed on the host machine and
the resulting relevant files are `copy`ed to the remote target machine.

  - `playbook.yml`
  ```yaml
  ---
  - name: Run role
    hosts: all
    roles:
      - role: generate-tls-certs
  ```

  - `inventory.yml`
  ```yaml
  ---
  all:
    hosts:
      srv1:
        ansible_host: 192.168.123.30
      srv2:
        ansible_host: 192.168.123.31
    vars:
      gen_tls_cert_dir: ./certs
      gen_tls_generate_ca_cert: true
      gen_tls_generate_client_cert: true
      gen_tls_generate_server_cert: true
      gen_tls_ca_email: me@example.org
      gen_tls_ca_country: EU
      gen_tls_ca_state: Italy
      gen_tls_ca_locality: Rome
      gen_tls_ca_organization: Example Inc.
      gen_tls_ca_organizationalunit: SysAdmins
      gen_tls_populate_etc_hosts: yes
  ```

If you want to tinker, you can use `vagrant` with the provided `Vagrantfile`.
It assumes `vagrant-libvirt` is installed (along with `libvirt`, of course).

Run it like this:

```
$ vagrant up --provider=libvirt --provision
```
