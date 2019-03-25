# Ansible Playbooks

## Inventory
The Inventory is located at `inventory/hosts.yml`. Run `cp hosts.example.yml hosts.yml` and edit `hosts.yml` to create an initial inventory of Ansible hosts.
Host variables are located in the `inventory/vars` folder.

### General Vars
- `admin_email`: Email address to forward mails for `root` to
- `postfix_smtp_server`: Postfix SMTP relay server
- `postfix_smtp_username`: Postfix SMTP relay username
- `postfix_smtp_password`: Postfix SMTP relay pasword
- `backblaze_account_id`: Required for off-site backups to Backblaze B2 cloud storage
- `backblaze_application_key`: Secret application key for Backblaze B2

### Host Vars
- `ansible_host`: Hostname or IP address
- `ansible_user`: Remote user
- `backup`: `"b2"` to enable off-site backup to Backblaze B2 cloud storage
- `sites`: See *Vhosts* section

## Playbooks

### Bootstrap
The `bootstrap.yml` playbook performs a base configuration of the server, including:
- Firewall
- Unattended Upgrades
- NTP
- Postfix
- Mosh
- Security/Hardening
- Backup

Hosts need to be members of the `bootstrap` inventory group.

```bash
ansible-playbook bootstrap.yml
```

### Stack
The `stack.yml` playbook installs a basic web stack on the server, including:
- Nginx
- Docker
- Certbot (for Let's Encrypt certificates)

Hosts need to be members of the `stack` inventory group.

```bash
ansible-playbook stack.yml
```

### DigitalOcean
The `digitalocean.yml` playbook uses the `inventory/vars/digitalocean.yml` file as a source for creating new DigitalOcean Droplets. After the Droplets are created, they are automatically stored in the `inventory/hosts.yml` file.

```bash
ansible-playbook digitalocean.yml
```

### VHost
The `vhosts.yml` playbook iterates over the `sites` host var. It will set-up a new Nginx virtual host for the hostnames in the `server_name` array. SSL certificates will be requested from *Let's Encrypt* and automatically renewed.

Hosts need to be members of the `vhost` inventory group.

```bash
ansible-playbook vhost.yml
```
