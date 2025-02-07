# Upgrades notes for the playbook

## Unreleased version

### Secrets management

Playbook has been enhanced to ease the adoption of Ansible Vault or third-party
lookup plugins to securely store secrets.

To be able to run the playbook with the new version, all the secrets needs to be
moved inside the `vars/secrets.yml` with a set of known keys:

```yml
repo_db_password: ""
sync_db_password: ""
reposearch_shared_secret: ""
activemq_password: ""
```

If you are managing a test environment and don't want to bother manually
configuring passwords, you can proceed as usual just by setting the
`autogen_unsecure_secrets` variable to `true` in `group_vars/all.yml`. In this
way secrets will be auto-generated but will be saved on your file-system in
plain text as in the previous versions of the playbook.

If you are managing a production environment and you want to reuse them same
credentials, fill the `vars/secrets.yml` accordingly and remove them from
`group_vars/all.yml` or anywhere you are providing them.

To enable secrets file encryption you can run the `secrets-init.yml` playbook,
that is also generating any missing secret automatically. The encryption is
using a password supplied manually before each run.

```bash
ansible-playbook --ask-vault-pass -e vault_init=encrypted_file playbooks/secrets-init.yml
```

From now on every standard `ansible-playbook` invocation should use the
`--ask-vault-pass` flag otherwise secrets decryption will fail.

To discover how to avoid having to provide manually a passphrase, use
third-party plugins and/or to learn all the new capabilities for secrets
management of the playbook please read the [Deployment guide](deployment-guide.md#secrets-management).
