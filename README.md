
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
