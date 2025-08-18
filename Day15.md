# Day 15: Deploy Nginx with SSL on App Server 3

## Question

The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 3 in Stratos Datacenter. They have some prerequisites to get ready that server for application deployment.

**Task:**  
1. Install and configure nginx on App Server 1.  
2. On App Server 1, there is a self-signed SSL certificate and key present at `/tmp/nautilus.crt` and `/tmp/nautilus.key`. Move them to an appropriate location and deploy the same in Nginx.  
3. Create an `index.html` file with content `Welcome!` under Nginx document root.  
4. For final testing, try to access the App Server 1 link (either hostname or IP) from jump host using curl command. For example:  
   `curl -Ik https://<app-server-ip>/`

---

## Solution

1. **Login to App Server 1**

   ```bash
   ssh banner@stapp03
   sudo su -
   ```

2. **Install Nginx and EPEL Repository**

   ```bash
   yum install epel-release -y
   yum install nginx -y
   ```

3. **Move SSL Certificate and Key to Appropriate Location**

   ```bash
   mkdir -p /etc/pki/CA/certs
   mkdir -p /etc/pki/CA/private
   cp /tmp/nautilus.crt /etc/pki/CA/certs/
   cp /tmp/nautilus.key /etc/pki/CA/private/
   ```

4. **Configure Nginx for SSL**

   Edit `/etc/nginx/nginx.conf` and add the following server block:

   ```nginx
   server {
       listen       443 ssl http2;
       server_name  172.16.238.12;
       root         /usr/share/nginx/html;

       ssl_certificate "/etc/pki/CA/certs/nautilus.crt";
       ssl_certificate_key "/etc/pki/CA/private/nautilus.key";
       ssl_session_cache shared:SSL:1m;
       ssl_session_timeout  10m;
       ssl_ciphers HIGH:!aNULL:!MD5;
       ssl_prefer_server_ciphers on;

       include /etc/nginx/default.d/*.conf;

       error_page 404 /404.html;
       location = /40x.html { }

       error_page 500 502 503 504 /50x.html;
       location = /50x.html { }
   }
   ```

5. **Create index.html with Required Content**

   ```bash
   rm -f /usr/share/nginx/html/index.html
   echo "Welcome!" > /usr/share/nginx/html/index.html
   ```

6. **Start and Enable Nginx**

   ```bash
   systemctl start nginx
   systemctl enable nginx
   systemctl status nginx
   ```

7. **Validate from Jump Host**

   ```bash
   curl -Ik https://stapp03
   ```

   **Expected Output:**

   ```
   HTTP/1.1 200 OK
   Server: nginx/1.20.1
   Content-Type: text/html
   Content-Length: 9
   ```

---

## Note

- Ensure firewall rules allow traffic on port 443.
- The SSL certificate and key must be readable by the nginx process.
- The document root should contain the `index.html` file with the content `Welcome!`