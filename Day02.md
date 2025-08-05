# Day 02: Temporary User Creation for Nautilus Project – Stratos Datacenter

## Question

To support limited-duration access for the Nautilus project, the system admin team at xFusionCorp Industries requires the creation of a temporary user account.

**Task:**  
Create a user named `javed` on App Server 1 in the Stratos Datacenter, with an expiry date of `2024-01-28`.

---

## Solution

```bash
ssh banner@stapp01
sudo useradd -e 2024-01-28 javed
```
Type the password for the user when prompted to complete the user creation process.

---

## Verification

To verify the expiry date for the user, run:

```bash
sudo chage -l javed
```

The output should show the account expiration date as `Jan 28, 2024`.

**Example Output:**

```text
Account expires    : Jan 28, 2024
```
