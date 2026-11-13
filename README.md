```sh
mkdir -p ~/Desktop/DevOps/2cloud-init
```

```sh
cd ~/Desktop/DevOps/2cloud-init
```

```sh
brew install --cask multipass
```

```sh
multipass version
```

```sh
nano cloud-config.yaml
```

```yaml
#cloud-config
package_update: true
packages:
  - nginx
  - python3-flask
  - postgresql
  - python3-psycopg2
  - redis-server
  - python3-redis
  - apache2-utils
write_files:
  - path: /var/www/html/index.html
    content: |
      Dela
runcmd:
  - systemctl enable --now nginx
  - systemctl enable --now postgresql
  - systemctl enable --now redis-server
```

```sh
multipass launch 26.04 --name devops-cloud --cpus 2 --memory 2G --disk 10G --cloud-init cloud-config.yaml
```

Po spremembah označi virtualko za izbris:
```sh
multipass delete devops-cloud
```

Dejansko izbriši virtualko:
```sh
multipass purge
```

Poženi na novo:
```sh
multipass launch 26.04 --name devops-cloud --cpus 2 --memory 2G --disk 10G --cloud-init cloud-config.yaml
```

# Pogon na šolski virtualki

- `sudo multipass launch --name devops --cloud-init cloud-init.yaml`
- `multipass info devops`
  - Pobereš ven `<LOKALNI IP>`
- `ip addr`
  - Pobereš ven javni IP `<JAVNI IP>` => `88.200.24.5`
- `sudo socat TCP-LISTEN:8080,bind=0.0.0.0,reuseaddr,fork TCP:<LOKALNI IP>:80`