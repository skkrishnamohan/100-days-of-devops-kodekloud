# Day 10: Automate Website Backup with Bash Script

## Question

The production support team of xFusionCorp Industries is working on developing bash scripts to automate different day-to-day tasks. One requirement is to create a bash script for taking website backups. They have a static website running on App Server 2 in Stratos Datacenter, and need a script named `blog_backup.sh` to accomplish the following tasks. The script should be placed under `/scripts` directory on App Server 2.

**Task:**  
- Create a zip archive named `xfusioncorp_blog.zip` of `/var/www/html/blog` directory.
- Save the archive in `/backup/` on App Server 2.
- Copy the created archive to Nautilus Backup Server in `/backup/` location.
- Ensure the script does not prompt for a password while copying the archive file.
- The respective server user (e.g., `steve` for App Server 2) must be able to run it.

---

## Solution

1. **Login to App Server 2**

   ```bash
   ssh steve@stapp02
   ```

2. **Create the Backup Script**

   ```bash
   vi /scripts/blog_backup.sh
   ```

   Example script content:

   ```bash
   #!/bin/bash
   zip -r /backup/xfusioncorp_blog.zip /var/www/html/blog
   scp /backup/xfusioncorp_blog.zip clint@stbkp01:/backup
   ```

   Make the script executable:

   ```bash
   chmod +x /scripts/blog_backup.sh
   ```

3. **Set Up Password-less SSH to Backup Server**

   Generate SSH key (if not already present):

   ```bash
   ssh-keygen
   ```

   Copy the public key to the backup server:

   ```bash
   ssh-copy-id clint@stbkp01
   ```

   Test SSH login to ensure no password is required:

   ```bash
   ssh clint@stbkp01
   ```

4. **Run the Backup Script**

   ```bash
   /scripts/blog_backup.sh
   ```

5. **Verify Backup**

   On App Server 2:

   ```bash
   ls -l /backup/xfusioncorp_blog.zip
   ```

   On Backup Server:

   ```bash
   ssh clint@stbkp01
   ls -l /backup/xfusioncorp_blog.zip
   ```

---

## Note

- Ensure the `/scripts` and `/backup` directories exist and have proper permissions.
- The script should be run by the respective server user (e.g., `steve`).
- Password-less SSH setup is required for seamless execution of the script.