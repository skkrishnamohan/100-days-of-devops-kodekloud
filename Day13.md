# Day 13: Secure Apache Port 8082 with iptables Firewall

## Question

The security team at xFusionCorp Industries has raised a concern that Apache’s port 8082 is open for all since there is no firewall installed on the app hosts in Stratos DC. To address this, a security layer needs to be added.

**Task:**  
- Install iptables and all its dependencies on each app host.  
- Block incoming port 8082 on all apps for everyone except for the LBR host (`172.16.238.14`).  
- Make sure the rules remain, even after system reboot.

---

## Solution

1. **Install iptables and Dependencies**

   ```bash
   sudo yum install -y iptables-services
   ```

2. **Start and Enable iptables Service**

   ```bash
   sudo systemctl start iptables
   sudo systemctl enable iptables
   ```

3. **Allow LBR Host and Block Others on Port 8082**

   ```bash
   sudo iptables -A INPUT -p tcp --dport 8082 -s 172.16.238.14 -j ACCEPT
   sudo iptables -A INPUT -p tcp --dport 8082 -j DROP
   ```

4. **Save the iptables Rules**

   ```bash
   sudo service iptables save
   ```

5. **Restart and Check Status**

   ```bash
   sudo systemctl restart iptables
   sudo systemctl status iptables
   ```

6. **Verify Rules**

   ```bash
   sudo iptables -L --line-numbers
   ```

---

## Note

- Repeat these steps for all app servers in Stratos DC.
- The rules will persist after a reboot because the iptables service is enabled and configured to restore the rules on startup.