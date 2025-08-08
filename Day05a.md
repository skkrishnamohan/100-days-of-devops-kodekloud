# Day 05a: Install and Permanently Disable SELinux

## Question

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App Server 1 in the Stratos Datacenter:

**Task:**  
- Install the required SELinux packages.  
- Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.  
- No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.  
- Disregard the current status of SELinux via the command line; the final status after the reboot should be disabled.

---

## Solution

### 1. Install SELinux Packages

For RHEL/CentOS-based systems:

```bash
sudo yum install -y selinux-policy selinux-policy-targeted
```

For Debian-based systems:

```bash
sudo apt install -y selinux selinux-utils selinux-policy-default
```

---

### 2. Permanently Disable SELinux

Edit the SELinux configuration file:

```bash
sudo vi /etc/selinux/config
```

Change the line:

```ini
SELINUX=enforcing
```

To:

```ini
SELINUX=disabled
```

This ensures SELinux will be disabled after the next reboot, regardless of its current runtime status.

---

### 3. Verify Configuration (Pre-Reboot)

Confirm the config file is set correctly:

```bash
grep ^SELINUX= /etc/selinux/config
```

**Expected Output:**

```text
SELINUX=disabled
```

---

## Note

- Do **not** reboot the server or run `setenforce 0` manually. The system will apply the change automatically on the next scheduled reboot.

---

## Summary

| Task                        | Status         |
|-----------------------------|---------------|
| SELinux packages installed  | ✅            |
| SELinux set to disabled     | ✅ (post-reboot) |
| Immediate reboot required   | ❌ (scheduled)   |
