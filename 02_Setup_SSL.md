
# Setting Up SSL with Let's Encrypt for a Bought Domain

This guide explains how to set up **SSL** for a bought domain using **Let's Encrypt** on a Google VM. The instructions will walk you through each step, including how to SSH into your VM and configure the SSL certificate.

## Prerequisites

- A **Google VM** running **Ubuntu** or any similar Linux-based OS.
- A **domain** bought from a domain registrar (e.g., **Namecheap**, **Google Domains**, etc.).
- **Nginx** installed on your VM.
- **Certbot** (Let’s Encrypt’s tool) installed for obtaining the SSL certificate.

## Step 1: SSH into Your Google VM

1. Open your terminal (or use a program like **PuTTY** if you're on Windows).
2. SSH into your VM:
   ```bash
   ssh username@<your-vm-ip>
   ```
   You'll be prompted for your password if you're logging in for the first time or if you've set one.
3. Ensure you are logged in as a non-root user (with `sudo` privileges) or as `root`.

## Step 2: Install Nginx (if not installed)

If **Nginx** is not installed on your VM, follow these steps:

1. Update package list:
   ```bash
   sudo apt update
   ```
2. Install Nginx:
   ```bash
   sudo apt install nginx -y
   ```
3. Start Nginx:
   ```bash
   sudo systemctl start nginx
   ```
4. Enable Nginx to start at boot:
   ```bash
   sudo systemctl enable nginx
   ```

## Step 3: Install Certbot for SSL Certificates

**Certbot** is the tool used to request and install the SSL certificate from **Let’s Encrypt**.

1. Install Certbot and Nginx plugin:
   ```bash
   sudo apt install certbot python3-certbot-nginx -y
   ```

## Step 4: Obtain an SSL Certificate with Let's Encrypt

1. Stop Nginx temporarily (Certbot needs to access port 80 for HTTP validation):
   ```bash
   sudo systemctl stop nginx
   ```
2. Run Certbot to get your SSL certificate:
   Replace `emi-survey.com` with your actual domain name:
   ```bash
   sudo certbot certonly --standalone -d emi-survey.com
   ```
   Certbot will create a temporary server to validate your domain and issue the certificate.
3. Check if the certificates were successfully created:
   After the process is complete, the certificates should be in `/etc/letsencrypt/live/emi-survey.com/`.

## Step 5: Configure Nginx to Use SSL

1. Start Nginx again:
   ```bash
   sudo systemctl start nginx
   ```
2. Open your Nginx configuration file:
   ```bash
   sudo nano /etc/nginx/sites-available/default
   ```
3. Edit the configuration to point to the **Let’s Encrypt SSL certificates**:
   Replace the existing SSL certificate paths with the following lines:
   ```nginx
   server {
       listen 443 ssl;
       server_name emi-survey.com;  # Replace with your domain

       # SSL Configuration (Let's Encrypt)
       ssl_certificate /etc/letsencrypt/live/emi-survey.com/fullchain.pem;
       ssl_certificate_key /etc/letsencrypt/live/emi-survey.com/privkey.pem;

       # SSL Protocols and Cipher Suite (Optional - Enhances Security)
       ssl_protocols TLSv1.2 TLSv1.3;
       ssl_prefer_server_ciphers on;
       ssl_ciphers HIGH:!aNULL:!MD5;

       # Proxy Requests to Flask (Potato App)
       location / {
           proxy_pass http://127.0.0.1:8001;  # Forward requests to Flask server
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }

   # Redirect HTTP to HTTPS
   server {
       listen 80;
       server_name emi-survey.com;  # Replace with your domain
       return 301 https://$host$request_uri;
   }
   ```
4. Save and Exit:
   Press `CTRL + O` to save.
   Press `CTRL + X` to exit.

## Step 6: Test Nginx Configuration

Before reloading Nginx, ensure the configuration is correct.

1. Test the configuration:
   ```bash
   sudo nginx -t
   ```
   You should see:
   ```
   nginx: configuration file /etc/nginx/nginx.conf syntax is ok
   nginx: configuration file /etc/nginx/nginx.conf test is successful
   ```
2. Reload Nginx:
   ```bash
   sudo systemctl reload nginx
   ```

## Step 7: Verify SSL Installation

1. Visit your domain in the browser:
   ```bash
   https://emi-survey.com
   ```
   You should see the **padlock** icon, indicating the site is secured with **SSL**.
2. Test HTTP to HTTPS redirection:
   - Visit `http://emi-survey.com` and check that it redirects to `https://emi-survey.com`.

## Step 8: Set Up Automatic SSL Renewal

Let’s Encrypt certificates are valid for **90 days**, so it's crucial to ensure automatic renewal is properly configured. Below is a complete setup for automatic and manual renewal using Certbot and Nginx.

###  1. Configure Automatic Renewal with Cron

Create a cron job for the root user that runs Certbot twice per day and reloads Nginx if a renewal occurs.

Open the root crontab:
```bash
sudo crontab -e
```

Add the following line to the bottom of the file:
```bash
0 3,15 * * * certbot renew --quiet && systemctl reload nginx
```

This runs at **3:00 AM and 3:00 PM** daily. If a certificate is due for renewal (typically within 30 days of expiry), Certbot will renew it, and Nginx will be reloaded to apply the new certificate.

To confirm the job was added:
```bash
sudo crontab -l
```

###  2. Test the Renewal Process

You can simulate a renewal to make sure everything works:
```bash
sudo certbot renew --dry-run
```

This tests the renewal process without actually changing any certificates.

---

### ️ 3. Manual Renewal (If Automatic Fails)

If automatic renewal fails or you're unsure it's working, you can manually renew the certificate.

#### Step A: Stop Nginx (required for standalone mode)
```bash
sudo systemctl stop nginx
```

#### Step B: Manually renew the certificate
```bash
sudo certbot renew
```

Or for specific domains:
```bash
sudo certbot certonly --standalone -d yourdomain.com
```

#### Step C: Start Nginx again
```bash
sudo systemctl start nginx
```

---

###  4. Check Certificate Expiration

To verify when your certificates expire:
```bash
sudo certbot certificates
```

This shows all managed certificates, their domains, and expiry dates.


### Troubleshooting Tips:
- **If SSL isn’t working**, check if the certificates exist in `/etc/letsencrypt/live/emi-survey.com/`.
- **Check Nginx error logs** for any issues:
  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```
