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

## Installation Steps

### Step 1: Install Nginx

1. **Update your package list:**
   ```bash
   sudo apt update
2. **Install Nginx:**
   ```bash
   sudo apt install nginx -y
3. **Start and enable Nginx to run on boot:**
   ```bash
   sudo systemctl start nginx
   sudo systemctl enable nginx
4. **Check the Nginx status:**
   ```bash
   sudo systemctl status nginx
   
### Step 2: Install Certbot and Obtain an SSL Certificate

1. **Install Certbot and the Nginx plugin:**
   ```bash
   sudo apt install certbot python3-certbot-nginx -y
2. **Obtain an SSL certificate for your domain: Replace domain_name with your actual domain.**
   ```bash
   sudo certbot --nginx -d domain_name
3. **Follow the instructions to set up SSL. Certbot will:**
   - Generate the certificate.
   - Update your Nginx configuration to use the SSL certificate.
4. **Test the certificate renewal process (optional):**
   ```bash
   sudo certbot renew --dry-run

### Step 3: Nginx Configuration

1. **Place the configuration file (nginx.conf) in the Nginx configuration directory, usually /etc/nginx/sites-available/ or /etc/nginx/conf.d/:**
   ```bash
   sudo nano /etc/nginx/sites-available/default
2. **Restart Nginx to apply changes:**
   ```bash
   sudo systemctl reload nginx

### Step 4: Verify SSL and Nginx Configuration
   - Access your domain over HTTPS (https://domain_name) and verify the SSL certificate is working.
   - Check that your application is accessible and proxied correctly to localhost:8080.

### How to Use
**Clone this repository:**
   ```bash
   git clone https://github.com/your-username/nginx-config.git
   cd nginx-config
**Place the nginx.conf file into your Nginx configuration directory (as described in Step 3 above) and restart Nginx.**

### Requirements
   - Nginx: Ensure Nginx is installed and properly configured (instructions provided above).
   - Certbot: Install Certbot for managing Let's Encrypt SSL certificates.
   - Application: A backend application running on localhost:8080.

### License
This project is open-source and can be used under the MIT License.

This updated `README.md` now includes detailed steps for installing Nginx, installing Certbot, and obtaining SSL certificates. It also provides instructions on how to place and use the configuration file.
