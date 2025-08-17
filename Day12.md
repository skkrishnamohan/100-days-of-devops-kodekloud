# Day 12: Fix Apache Service Unreachability on App Server

## Question

Our monitoring tool has reported an issue in Stratos Datacenter. One of our app servers has an issue, as its Apache service is not reachable on port 8085 (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue. Use tools like telnet, netstat, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

**Task:**  
Identify and fix the issue preventing Apache from being reachable on port 8085 from the jump host. Ensure Apache is running and accessible.

---

## Solution

1. **Login to the Faulty App Server**

   ```bash
   ssh tony@stapp01
   sudo su -
   ```

2. **Check Apache Service Status**

   ```bash
   systemctl status httpd
   ```

   If the status is inactive, find the error:

   ```bash
   httpd -t
   ```

3. **Edit Apache Configuration**

   Open the configuration file:

   ```bash
   vi /etc/httpd/conf/httpd.conf
   ```

   Uncomment and set the `ServerName` directive:

   ```
   ServerName 172.16.238.10:8085
   ```

   Save and exit (`:wq!`).

4. **Check for Port Conflicts**

   ```bash
   netstat -anp | grep LISTEN | grep ":8085"
   ```

   If another process is using port 8085, kill it:

   ```bash
   kill -9 <pid_number>
   ```

5. **Restart Apache**

   ```bash
   systemctl restart httpd
   systemctl status httpd
   ```

6. **Update Firewall Rules**

   Add a rule to allow traffic on port 8085:

   ```bash
   sudo iptables -I INPUT -p tcp -m tcp --dport 8085 -j ACCEPT
   sudo iptables-save > /etc/sysconfig/iptables
   cat /etc/sysconfig/iptables
   ```

7. **Verify from Jump Host**

   From the jump host, test connectivity:

   ```bash
   curl http://stapp01:8085
   ```

---

## Note

- Adjust the port number according to the task requirements.
- Ensure you do not compromise any existing security settings while making Apache reachable.
- Use `netstat`, `telnet`, and `curl` to troubleshoot and verify