# Day 07: Set Up Password-less SSH Access from Jump Host

## Question

The system admins team of xFusionCorp Industries has set up scripts on the jump host that run at regular intervals and perform operations on all app servers in the Stratos Datacenter. To make these scripts work properly, the `thor` user on the jump host needs password-less SSH access to all app servers through their respective sudo users (i.e., `tony` for app server 1, `steve` for app server 2, and `banner` for app server 3).

**Task:**  
Set up password-less authentication from user `thor` on the jump host to all app servers through their respective sudo users.

---

## Solution

1. **Generate SSH Key on Jump Host**

   ```bash
   ssh-keygen -t rsa -b 4096
   ```
   - Press Enter to accept the default location (`~/.ssh/id_rsa`)
   - Leave the passphrase empty for automation

2. **Copy Public Key to App Servers**

   ```bash
   # App Server 1
   ssh-copy-id tony@app_server_1

   # App Server 2
   ssh-copy-id steve@app_server_2

   # App Server 3
   ssh-copy-id banner@app_server_3
   ```

---

## Verification

From `thor@jump_host`, test each connection:

```bash
ssh tony@app_server_1
ssh steve@app_server_2
ssh banner@app_server_3
```

You should be able to log in without entering a password.

---

## Secure Permissions (Optional but Recommended)

On each app server, ensure correct permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## Note

- Ensure the SSH service is running on all app servers.
- The public key from `thor@jump_host` will be added to the `~/.ssh/authorized_keys` file of each sudo user on the app servers.