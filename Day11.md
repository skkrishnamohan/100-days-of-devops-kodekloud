# Day 11: Deploy Java Application on Tomcat Server

## Question

The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the Tomcat application server.

**Task:**  
- Install Tomcat server on App Server 3.  
- Configure it to run on port 8086.  
- There is a `ROOT.war` file on the Jump host at location `/tmp`. Deploy it on this Tomcat server and make sure the webpage works directly on the base URL, i.e., `curl http://stapp03:8086`.

---

## Solution

1. **Copy the WAR File from Jump Host to App Server**

   ```bash
   scp /tmp/ROOT.war banner@stapp03:/tmp
   ```

2. **Login to App Server 3 and Switch to Root**

   ```bash
   ssh banner@stapp03
   sudo su -
   ```

3. **Install Tomcat Packages**

   ```bash
   yum install -y tomcat
   ```

4. **Edit Tomcat Configuration to Change Port**

   Open the Tomcat config file:

   ```bash
   vi /usr/share/tomcat/conf/server.xml
   ```

   Find the non-SSL HTTP/1.1 Connector and set the port to `8086`:

   ```xml
   <Connector port="8086" protocol="HTTP/1.1" ... />
   ```

   Save and exit.

5. **Deploy the WAR File**

   ```bash
   cp /tmp/ROOT.war /usr/share/tomcat/webapps/
   ```

6. **Start Tomcat Service**

   ```bash
   systemctl start tomcat
   systemctl status tomcat
   ```

   Ensure the service is `active (running)`.

7. **Validate Deployment**

   ```bash
   curl -i http://stapp03:8086
   ```

   You should see a `200 OK` response and the application page.

---

## Note

- Make sure the Tomcat service is enabled to start on boot if required:
  ```bash
  systemctl enable tomcat
  ```
- If you encounter port conflicts, use `netstat -tulnp | grep 8086` to identify and resolve them.
- The application should be accessible directly at the base URL