# Day 08: Install Ansible 4.9.0 on Jump Host Using pip3

## Question

During the weekly meeting, the Nautilus DevOps team discussed automation and configuration management solutions. After considering several options, the team decided to use Ansible due to its simple setup and minimal prerequisites. They want to start testing using Ansible, so the jump host will be used as an Ansible controller to run tasks on the other servers.

**Task:**  
Install Ansible version 4.9.0 on the jump host using `pip3` only. Make sure the Ansible binary is available globally on this system, so all users can run Ansible commands.

---

## Solution

1. **Install pip3 (if not already installed)**

   For Ubuntu/Debian systems:
   ```bash
   sudo apt update
   sudo apt install -y python3-pip
   ```

   For RHEL/CentOS systems:
   ```bash
   sudo yum install -y python3-pip
   ```

2. **Install Ansible 4.9.0 using pip3**

   ```bash
   sudo pip3 install ansible==4.9.0
   ```

   This installs Ansible and its dependencies system-wide using `sudo`, ensuring it's available to all users.

3. **Verify Installation**

   ```bash
   ansible --version
   ```

   **Expected Output:**
   ```
   ansible [core 2.11.x]
     config file = ...
     ...
   ```
   > **Note:** Ansible 4.9.0 is part of the Ansible community package, which includes ansible-core 2.11.x.

4. **Ensure Global Availability**

   Since we used `sudo pip3 install`, the Ansible binary is placed in a system-wide location like `/usr/local/bin`, which is typically in every user's `$PATH`.

   Confirm this with:
   ```bash
   which ansible
   ```

   **Expected Output:**
   ```
   /usr/local/bin/ansible
   ```

5. **Test Across Users**

   To confirm global access:
   ```bash
   sudo su - anotheruser
   ansible --version
   ```

---

## Note

- Using `sudo pip3 install` ensures Ansible is available to all users.
- Ansible 4.9.0 is not available via OS package managers, so pip3 is required for this version.