# Nginx Configuration for Reverse Proxy with SSL

This repository contains an Nginx server configuration that acts as a reverse proxy with SSL enabled using Let's Encrypt. The configuration forwards requests to an application running on `localhost:8080` and serves them over HTTPS.

## Configuration Details

### Server Block 1: HTTPS (Port 443)

- **Domain:** `domain_name`
- **Proxy Pass:** Requests are forwarded to `http://127.0.0.1:8080`.
- **SSL:** Managed by Certbot using Let's Encrypt certificates.
- **Headers:** Includes headers for upgrading WebSocket connections and forwarding proxy details.

### Server Block 2: HTTP to HTTPS Redirection (Port 80)

- Redirects all HTTP traffic to HTTPS.
- If the domain name is accessed via HTTP, the configuration issues a `301` redirect to the HTTPS version of the site.

## Key Configuration Parameters

### Proxy Headers
The following headers are set to manage forwarding of client details and WebSocket upgrades:
- `Upgrade` and `Connection`: For handling WebSocket connections.
- `Host`, `X-Real-IP`, `X-Forwarded-For`: Forward the original client IP and host information.
- `X-Forwarded-Proto`, `X-Forwarded-Host`, `X-Forwarded-Port`: Provide additional forwarding information to the backend.

### Proxy Timeouts
Timeouts are set to 60 seconds for connection, send, and read operations:
- `proxy_connect_timeout 60s;`
- `proxy_send_timeout 60s;`
- `proxy_read_timeout 60s;`

### SSL Configuration
SSL is handled by Certbot with the following directives:
- `ssl_certificate`: Path to the fullchain certificate.
- `ssl_certificate_key`: Path to the private key.
- The configuration includes strong SSL options and Diffie-Hellman parameters for enhanced security.

### Let's Encrypt Configuration
Certbot manages SSL certificates, with automatic renewal and security options applied:
- `include /etc/letsencrypt/options-ssl-nginx.conf;` - Default SSL options provided by Certbot.
- `ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;` - Strong Diffie-Hellman group for key exchange.

## How to Use

1. Place the configuration file (`nginx.conf`) in the Nginx configuration directory, usually `/etc/nginx/sites-available/` or `/etc/nginx/conf.d/`.
2. Ensure you have Certbot installed and have obtained an SSL certificate for your domain.
3. Reload or restart Nginx to apply the configuration:
   ```bash
   sudo systemctl reload nginx
4. Verify that your application is accessible over HTTPS and traffic is correctly proxied to localhost:8080.

## Requirements

- Nginx: Ensure Nginx is installed and properly configured.
- Certbot: Install Certbot for managing Let's Encrypt SSL certificates.
- Application: A backend application running on localhost:8080.

## License
This project is open-source and can be used under the MIT License.

This `README.md` explains the purpose of the configuration, breaks down its sections, and provides instructions on how to use it. It also highlights the key features, such as proxy headers, timeouts, and SSL setup.

