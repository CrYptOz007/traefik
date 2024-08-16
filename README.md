
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
