# Ansible Playbook: Laravel on DigitalOcean

Installs and configures a [Laravel](https://laravel.com/) and [MariaDB](https://mariadb.org) droplet on DigitalOcean using [Ansible](https://ansible.com) playbooks.

## Requirements

Install `passlib` for hashing the admin user password.

```bash
$ python -m pip install passlib --user
```

## Role Variables

### Swap

See [geerlingguy.swap](https://github.com/geerlingguy/ansible-role-swap) for more info.

### MariaDB

See [geerlingguy.mysql](https://github.com/geerlingguy/ansible-role-mysql) for more info.

### Redis

See [geerlingguy.redis](https://github.com/geerlingguy/ansible-role-redis) for more info.

### PHP

See [geerlingguy.php](https://github.com/geerlingguy/ansible-role-php) for more info.

### Composer

See [geerlingguy.composer](https://github.com/geerlingguy/ansible-role-composer) for more info.

### Node.js

Uses version `lts/carbon`.

See [geerlingguy.nodejs](https://github.com/geerlingguy/ansible-role-nodejs) for more info.

### Nginx

Nginx details are stored in `vars/web.yml` and `web.yml`.

See [geerlingguy.nginx](https://github.com/geerlingguy/ansible-role-nginx) for more info.

### Certbot

See [geerlingguy.certbot](https://github.com/geerlingguy/ansible-role-certbot) for more info.

### Laravel

Review [web.yml](./web.yml) to see how Laravel is deployed based on the vars in `group_vars/all.yml` and `vars/web.yml`.

### Common
* [swap](https://github.com/geerlingguy/ansible-role-swap)

### Web
* [git](https://github.com/geerlingguy/ansible-role-git)
* [redis](https://github.com/geerlingguy/ansible-role-redis)
* [php](https://github.com/geerlingguy/ansible-role-php)
* [php-mysql](https://github.com/geerlingguy/ansible-role-php-mysql)
* [composer](https://github.com/geerlingguy/ansible-role-composer)
* [node](https://github.com/geerlingguy/ansible-role-nodejsjs)
* [nginx](https://github.com/geerlingguy/ansible-role-nginx)
* [certbot](https://github.com/geerlingguy/ansible-role-certbot)

### Database
* [mysql](https://github.com/geerlingguy/ansible-role-mysql) (MariaDB)

## Example Playbook

```yml
---
- name: Setup a preconfigured Laravel and MySQL droplet on DigitalOcean
  role:
    - stedding
```

## Run the Playbook

Clone the project

```bash
$ git clone https://github.com/cwilby/ansible-role-laravel-do
```

Create a vault using [ansible-vault](https://docs.ansible.com/ansible/2.6/user_guide/vault.html) and enter secret values.

```bash
$ EDITOR=nano ansible-vault create group_vars/vault.yml
```

```yml
---
vault_app_name: acme
vault_app_tld: .com
vault_app_repo_url: https://github.com/laravel/laravel.git  # your repo here
vault_app_repo_branch: master
vault_upassword: changeme
vault_github_token: <https://github.com/settings/tokens>
vault_do_api_token: <https://cloud.digitalocean.com/account/api/tokens>
vault_web_droplet_ssh_pk: <https://cloud.digitalocean.com/account/security#keys>
vault_db_droplet_ssh_pk: <https://cloud.digitalocean.com/account/security#keys>
vault_github_keys: https://github.com/<yourusername>.keys
vault_web_droplet_region: sfo2
vault_db_droplet_region: sfo2
```

Install [ansible-galaxy](https://galaxy.ansible.com) dependencies.

```
$ ansible-galaxy install -r requirements.yml
```

Finally, run the playbook.

```bash
$ ANSIBLE_HOST_KEY_CHECKING=false ansible-playbook -i inventory --ask-become --ask-vault-pass playbook.yml
```

> `ANSIBLE_HOST_KEY_CHECKING` is set to false to turn off host key check for new droplets.