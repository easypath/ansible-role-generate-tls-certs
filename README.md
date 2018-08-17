Generate TLS certificates
=========================
Generates self-signed CA, client and server certificates. Runs locally on control machine.

Notes:
- Will not overwrite any files in output cert dir
- Ansible crypto modules do not support signing certs with own CA yet, using `shell` command instead. Should be resolved in Ansible 2.7 using the [ownca provider](https://github.com/ansible/ansible/commit/b61b113fb9e3fcfcb25f4a8aaabad618e3209ce1).


Requirements
------------
- For server certificates, must specify Ansible inventory file; FQDN must also be set as hostname in inventory file


Role Variables
--------------
See `defaults/main.yml`


Dependencies
------------
- Refer to [Ansible Crypto modules](http://docs.ansible.com/ansible/latest/modules/list_of_crypto_modules.html)


Example Playbook
----------------
**generate-certs.yaml:**
```
---

# ansible-playbook generate-certs.yaml -i localhost,
# ansible-playbook generate-certs.yaml -i inventory.yaml

- hosts: all

  gather_facts: false

  tasks:
    - include_vars: vars.yaml

    - name: Generate certs
      import_role:
        name: generate-tls-certs

```

**vars.yaml:**
```
---
  cert_dir: ./certs
  generate_ca_cert: true
  generate_client_cert: true
  generate_server_cert: true

  # -------
  # CA CERT
  # -------
  tls_ca_cert: my-ca.pem
  tls_ca_csr: my-ca.csr
  tls_ca_key: my-ca.key
  tls_ca_country: CA
  tls_ca_state: Ontario
  tls_ca_locality: Toronto
  tls_ca_organization: My Company Inc.
  tls_ca_organizationalunit: IT
  tls_ca_commonname: My Certificate Authority

  # -----------
  # CLIENT CERT
  # -----------
  tls_client_cert: my-client.pem
  tls_client_key: my-client.key
  tls_client_csr: my-client.csr
  tls_client_commonname: My Client

```


License
-------
BSD


Author Information
------------------
[EasyPath IT Solutions Inc.](https://www.easypath.ca)
