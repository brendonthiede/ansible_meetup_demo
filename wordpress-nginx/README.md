## WordPress+Nginx+PHP-FPM+MariaDB Deployment

#### Copied from [https://github.com/ansible/ansible-examples]()

### Requirements
- Requires Ansible 1.2 or newer
- Expects CentOS/RHEL 7.x host/s

#### OS Specifics

RHEL7 version reflects changes in Red Hat Enterprise Linux and CentOS 7:

1. Network device naming scheme has changed
2. iptables is replaced with firewalld
3. MySQL is replaced with MariaDB

These playbooks deploy a simple all-in-one configuration of the popular
WordPress blogging platform and CMS, frontend by the Nginx web server and the
PHP-FPM process manager.

Running the playbook will apply it to all servers marked as part of the `[wordpress-server]`
group.  Inventory file path can be defined in `~/.ansible.cfg`.  Sample inventory contents:

    [wordpress-server]
    myvm ansible_user=root ansible_port=1234 ansible_host=12.34.56.7

For more on managing your inventory of servers, see here:
[http://docs.ansible.com/ansible/intro_inventory.html]().
To see what servers are in that group, run:

    ansible-playbook site.yml --list-hosts

To actually apply the playbook, run:

    ansible-playbook site.yml -e@group_vars/vault --vault-password-file=~/.vault_pass.txt

The playbooks will configure MariaDB, WordPress, Nginx, and PHP-FPM. When the run
is complete, you can hit access server to begin the WordPress configuration.

### Configuration

You need to update the configuration in `group_vars/all` such as setting the database username
and password.

#### Generating and Storing Secrets

For secrets management, you should set up a password file named `.vault_pass.txt` in your
home directory (this should _not_ be in source control).
For adding secrets to the file, you can decrypt the file by
running:

    ansible-vault decrypt --vault-password-file=~/.vault_pass.txt secrets.yml

After modifying the file, re-encrypt it with:

    ansible-vault encrypt --vault-password-file=~/.vault_pass.txt secrets.yml

Make sure not to commit the file while unencrypted.  More here: [http://docs.ansible.com/ansible/playbooks_vault.html]()

#### Updating the WordPress version

If the version of WordPress changes, you can get the new sha256sum
on a Mac by running, for example:

    shasum -a 256 ~/Downloads/tmp/wordpress-4.7.3.tar.gz

