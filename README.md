# 100-days-of-devops-kodekloud

## Day 01: Create a User with Non-Interactive Shell

To accommodate the backup agent tool's specifications, the system admin team at xFusionCorp Industries requires the creation of a user with a non-interactive shell.

**Task:**  
Create a user named `ravi` with a non-interactive shell on App Server 3.

---

## Day 02: Temporary User Creation for Nautilus Project – Stratos Datacenter

To support limited-duration access for the Nautilus project, the system admin team at xFusionCorp Industries requires the creation of a temporary user account.

**Task:**  
Create a user named `javed` on App Server 1 in the Stratos Datacenter, with an expiry date of `2024-01-28`.

---

## Day 03: Disable Direct SSH Root Login

Following security audits, the xFusionCorp Industries security team has rolled out new protocols, including the restriction of direct root SSH login.

**Task:**  
Disable direct SSH root login on all app servers within the Stratos Datacenter.

---

## Day 04: Grant Executable Permissions to Backup Script

In a bid to automate backup processes, the xFusionCorp Industries sysadmin team has developed a new bash script named `xfusioncorp.sh`. While the script has been distributed to all necessary servers, it lacks executable permissions on App Server 1 within the Stratos Datacenter.

**Task:**  
Grant executable permissions to the `/tmp/xfusioncorp.sh` script on App Server 1. Ensure that all users have the capability to execute this script.

---

## Day 05: Create a Service User Without Home Directory

In response to the latest tool implementation at xFusionCorp Industries, the system admins require the creation of a service user account.

**Task:**  
Create a user named `mark` on App Server 2 in the Stratos Datacenter. This user should not have a home directory, as per the tool's requirements.

---

## Day 05a: Install and Permanently Disable SELinux

Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for App Server 1 in the Stratos Datacenter.

**Task:**  
- Install the required SELinux packages.  
- Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.  
- No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.  
- Disregard the current status of SELinux via the command line; the final status after the reboot should be checked.

---

## Day 06: Schedule a Cron Job for Root User

The Nautilus system admins team has prepared scripts to automate several day-to-day tasks. They want them to be deployed on all app servers in Stratos DC on a set schedule. Before that, they need to test similar functionality with a sample cron job.

**Task:**  
- Install the `cronie` package on all Nautilus app servers and start the `crond` service.  
- Add a cron job for the root user:  
  `*/5 * * * * echo hello > /dev/null 2>&1`

---

## Day 07: Set Up Password-less SSH Access from Jump Host

The system admins team of xFusionCorp Industries has set up scripts on the jump host that run at regular intervals and perform operations on all app servers in the Stratos Datacenter. To make these scripts work properly, the `thor` user on the jump host needs password-less SSH access to all app servers through their respective sudo users (i.e., `tony` for app server 1, `steve` for app server 2, and `banner` for app server 3).

**Task:**  
Set up password-less authentication from user `thor` on the jump host to all app servers through their respective sudo users.
