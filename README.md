
# Traefik 3

## Setup

To setup this project.

1. Required
```bash
  chmod 600 secrets/acme.json
```

2. Make a copy of environment file
```bash
cp .env.example .env
chmod 600 .env
```

3. Edit any services in
```bash
rules/app-*.yml
```

Some examples have already been given. `REPLACE_HERE` requires you to replace it with either the application's internal IP/Port on your network or the docker's instance alias name if it's running on the same `traefik_proxy` network. See `examples/portainer` and `app-portainer-yml` for setting up docker instances on the same network with traefik

Services that allow you to upload your own SSL Certificate (manually generating one through Cloudflare's Origin Server), you can set the option
```yml
      tls: {}
```
For services that don't have the ability to have an SSL certificate such as those not running via NGINX, ensure

```yaml
      tls:
        certResolver: dns-cloudflare
        options: letsencrypt
```
is set for the service to allow certificates to be auto generated for the hostname using letsencrypt.

4. Port forward `443/TCP` to your traefik's instance IP

## Environment Variables

To run this project, you will need to change the following environment variables to your `.env` file after you make a copy from `.env.example`
```
USERDIR="/home/dockerce" # Replace with your home directory
DOCKERDIR="/home/dockerce/traefik" # Replace with where this project is located
DOMAINNAME_CLOUD_SERVER= #EXAMPLE example.com
CLOUDFLARE_EMAIL= 
CLOUDFLARE_API_KEY= 
WAN_IP= #EXAMPLE 100.100.100.100/32
```

## Cloudflare
A domain is needed that's linked to Cloudflare by pointing the Nameservers

1. Obtain the Global API Key in My Profile > API Tokens and place in the .env file for `CLOUDFLARE_API_KEY`

2. Open the site's domain dashboard on Cloudflare and go to SSL/TLS > Origin Server

3. Click `Create Certificate` and leave the default settings

4. Create the certificate and private key in `certs` folder called `DOMAIN.pem` and `DOMAIN.key` which corresponds to the certificate and private key generated from creating the certificate

5. Download the authenticated pull certificate into certs.
```bash
curl --ouput certs/authenticated_origin_pull_ca.pem https://developers.cloudflare.com/ssl/static/authenticated_origin_pull_ca.pem
```

6. Set the required permissions
```bash
chmod 644 certs/*.pem
chmod 600 certs/*.pem
```

7. Go to the site's domain dashboard again and go to DNS > Records

8. Add an A Record with `@` for Name and your WAN IP as the `IPv4`. Ensure `Proxy status` is checked

9. Add a CNAME Record with `*` for Name and your domain for target. Ensure `Proxy status is checked

### Recommended Options

1. SSL/TLS > Overview - Encryption Mode `Full`
2. SSL/TLS > Edge Certificates - Always Use HTTPS `Checked`
3. SSL/TLS > Edge Certificates - Minimum TLS Version - `TLS 1.2`
4. SSL/TLS > Edge Certificates - Opportunistic Encryption `Checked`
5. SSL/TLS > Edge Certificates - TLS 1.3 `Checked`
6. SSL/TLS > Edge Certificates - Automatic HTTPS Rewrites `Checked`
7. SSL/TLS > Origin Server - Authenticated Origin Pulls `Checked`
