# Day 06: Schedule a Cron Job for Root User

## Question

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that, they need to test similar functionality with a sample cron job.

**Task:**  
- Install the `cronie` package on all Nautilus app servers and start the `crond` service.  
- Add a cron job for the root user:  
  `*/5 * * * * echo hello > /tmp/cron_text`

---

## Solution

1. **Login to Each App Server**

   For RHEL/CentOS-based systems:

   ```bash
   sudo yum install -y cronie
   sudo systemctl enable crond
   sudo systemctl start crond
   sudo systemctl status crond
   ```

   For Debian-based systems:

   ```bash
   sudo apt install -y cron
   sudo systemctl enable cron
   sudo systemctl start cron
   sudo systemctl status cron
   ```

2. **Add the Cron Job for Root**

   Edit the root user's crontab:

   ```bash
   sudo crontab -e
   ```

   Add the following line:

   ```cron
   */5 * * * * echo hello > /tmp/cron_text
   ```

   Save and exit (`:wq` in `vi`).

---

## Verification

After 5 minutes, check the output file:

```bash
cat /tmp/cron_text
```

**Expected Output:**

```text
hello
```

---

## Note

- Ensure the `crond` (or `cron`) service is running for the job to execute.
- The job will overwrite `/tmp/cron_text` with `hello` every 5 minutes.
- For help with cron syntax,visit [crontab.guru](https://crontab.guru/)