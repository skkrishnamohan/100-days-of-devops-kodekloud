# Day 14: Fix Apache Service Unavailability on App Server

## Question

The production support team of xFusionCorp Industries has deployed monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported Apache service unavailability on one of the app servers in Stratos DC.

**Task:**  
Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 8087 on all app servers.

---

## Solution

1. **Identify the Faulty App Server**

   From the jump host, check connectivity to each app server on port 8087:

   ```bash
   telnet stapp01 8087
   telnet stapp02 8087
   telnet stapp03 8087
   ```

   If you get `Connection refused` for a server, that's the faulty host.

2. **Login to the Faulty App Server**

   ```bash
   ssh tony@stapp01
   sudo su -
   ```

3. **Check Apache Service Status**

   ```bash
   systemctl start httpd
   systemctl status httpd
   ```

   If the service fails to start, check the error logs:

   ```bash
   journalctl -xe
   systemctl status httpd
   ```

4. **Check Which Service is Using Port 8087**

   ```bash
   netstat -tulnp | grep 8087
   ```

   If another service (e.g., `sendmail`) is using port 8087, note its PID.

5. **Stop the Conflicting Service**

   ```bash
   ps -ef | grep sendmail
   kill <PID>
   ```

6. **Start Apache Service**

   ```bash
   systemctl start httpd
   systemctl status httpd
   ```

   You should see `active (running)`.

7. **Validate Apache HTTPd Running**

   From the jump host, test again:

   ```bash
   telnet stapp01 8087
   ```

   You should now be able to connect.

---

## Note

- Always check which process is using the required port before starting Apache.
- Use `netstat` and `ps` to identify and stop conflicting services.
- Ensure Apache is enabled to start on boot if required:

  ```bash
  systemctl enable httpd
  ```
