## If Certificates are not Renewing as Expected ##
Assuming you started the container with `postal` and you are a member of the `docker` group,
enter the container by running
```
postal enter certbot
```

To quickly check certificate expiration dates, run

```
certbot certificates
```

Within the container, Let's Encrypt data is stored at the standard
location of `/etc/letsencrypt`. If the certificates are up to date but have not been deployed
to Heroku, you can update them manually using the Heroku CLI. For example given a certificate
name `mycert` and an app named `myapp`

```
cd /etc/letsencrypt/live/mycert
$heroku certs:update -a myapp --confirm myapp fullchain.pem privkey.pem
```
