# Day 09: Fix MariaDB Initialization Issue on Database Server

## Question

There is a critical issue going on with the Nautilus application in Stratos DC. The production support team identified that the application is unable to connect to the database. After investigation, the team found that the MariaDB service is down on the database server.

**Issue:**  
MariaDB is not initialized, but the directory `/var/lib/mysql` is not empty, so initialization cannot be done.  
MariaDB expects an empty data directory during initialization, but it already contains files—likely from a previous install or failed setup.

---

## Solution

> ⚠️ **Warning:**  
> Before proceeding, ensure that no critical data is in `/var/lib/mysql`. If this is a fresh setup or test environment, it's safe to clear. If it's production, backup first.

1. **Check for Existing Data**

   ```bash
   ls -l /var/lib/mysql
   ```
   If you see files like `ibdata1`, `ib_logfile0`, or `.frm` files, MariaDB thinks it's already initialized.

2. **Stop the Service (if running)**

   ```bash
   sudo systemctl stop mariadb
   ```

3. **Backup Existing Data (if needed)**

   ```bash
   sudo mv /var/lib/mysql /var/lib/mysql.bak
   sudo mkdir /var/lib/mysql
   sudo chown mysql:mysql /var/lib/mysql
   ```
   This preserves the old data in case you need to recover it later.

4. **Initialize the Database Directory**

   ```bash
   sudo /usr/libexec/mariadb-prepare-db-dir
   ```
   This script sets up the required files and permissions for MariaDB to start.

5. **Start the MariaDB Service**

   ```bash
   sudo systemctl start mariadb
   ```

6. **Enable on Boot (Optional)**

   ```bash
   sudo systemctl enable mariadb
   ```

7. **Verify Status**

   ```bash
   sudo systemctl status mariadb
   ```
   You should see `active (running)`.

---

## Verification

Try connecting to MariaDB:

```bash
mysql -u root
```

Check logs:

```bash
sudo journalctl -u mariadb
```

---

## Note

- Always backup existing data before making changes to the database directory.
- If this is a production environment, consult with your DBA or follow your organization's change management procedures.