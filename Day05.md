# Day 05: Create a Service User Without Home Directory

## Question

In response to the latest tool implementation at xFusionCorp Industries, the system admins require the creation of a service user account.

**Task:**  
Create a user named `mark` in App Server 3 without a home directory.

---

## Solution

1. **Login to App Server 3**

   ```bash
   ssh banner@stapp03
   ```

2. **Create the User Without a Home Directory**

   ```bash
   sudo useradd -M -d /nonexistent mark
   ```

   - `-M`: Do not create a home directory
   - `-d /nonexistent`: Explicitly set a non-existent home path to avoid `/home/mark`
   - `mark`: The username

   > **Note:**  
   > Sometimes using `-M` alone may still create a home folder, depending on Linux distribution defaults. Explicitly setting `-d /nonexistent` ensures no home directory is created.

3. **(Optional) Make the User Non-Login**

   If you want to prevent the user from logging in, use:

   ```bash
   sudo useradd -M -d /nonexistent -s /usr/sbin/nologin mark
   ```

---

## Verification

To verify the user and home directory, run:

```bash
getent passwd mark
```

**Expected Output:**

```text
mark:x:1001:1001::/nonexistent:/usr/login
```

---

## Note

Some Linux distributions (especially with custom `/etc/login.defs`) may override defaults unless you explicitly set the home directory path with `-d`. Even if you use `-M`, the system might still create `/home/mark` unless `-d` is specified.

