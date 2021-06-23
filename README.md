# Ubuntu Server Bootstrapper

> This isn't a very advanced script, it's just some help so that you don't
> have to do the things you've already done a thousand times before.

Ubuntu Server Bootstrapper (usb.sh) is a small shell script made for a faster
installation and setup of an Ubuntu server.

This script is designed for nodejs projects,
but can be used for webservers no-matter the language!

## Install

> Always check script's from the internet before running
> them, especially if they require root!

On Ubuntu as root, Run:

```bash
wget https://raw.githubusercontent.com/Jrp0h/usb/master/usb.sh

chmod +x usb.sh

./usb.sh
```

Then just follow the instructions!

## What does Ubuntu Server Bootstrapper do?

It installs programs(list of programs can be found below) and
configures nginx, ssh and a firewall(ufw).

It allows you to import ssh keys and create ssl certificates
with certbot from Let's Encrypt.

usb.sh will also prompt you too optionally create a user which will own
the files for the webapp(or whatever) that you will have on this server,
as well as own the ssh deploy keys which will automatically get generated
(rsa - 4096 bit).

## Generated Configurations

### NGINX

nginx will be configured as a reverse proxy.

The file

> /etc/nginx/sites-available/default

will get the content:

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    charset utf-8;

    server_name _;

    location / {
        proxy_pass http://localhost:3000/;
    }

}
```

### SSH

ssh will disallow password login so it requires ssh keys

The file

> /etc/ssh/sshd_config

will get the content:

```sshd_config
Include /etc/ssh/sshd_config.d/*.conf
PermitRootLogin no
ChallengeResponseAuthentication no
UsePAM no
X11Forwarding yes
PrintMotd no
AcceptEnv LANG LC_*
Subsystem sftp /usr/lib/openssh/sftp-server
PasswordAuthentication no
```

## Software the script installs

### Programs

All the programs and why they are needed.
Of course more programs and libraries have been install,
but these are the once that are explicitly downloaded and installed.

- dialog - UI for the script
- OpenSSH - For remote access
- nginx - Webserver
- MariaDB - Database
- Redis - Cache
- ufw - Firewall
- fail2ban - Security
- npm - Good for NodeJS
- node v14 lts - Good for NodeJS
- snap - Dependency of certbot
- Certbot (Let's Encrypt) - SSL certificate for webserver
