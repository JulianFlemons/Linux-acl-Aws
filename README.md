# 🛡️ Linux Company Access Control System Deployment (AWS EC2)

## I. Project Overview
This project demonstrates the deployment and configuration of a robust, multi-user Linux access control system on a cloud-hosted Ubuntu server. The entire system is built upon the **Principle of Least Privilege (PoLP)** to ensure strict data separation between departments.

* **Objective:** Implement a secure file-sharing structure where employees can only read and write data within their specific departmental share, and are strictly blocked from all others.
* **Platform:** AWS EC2 (Ubuntu 22.04 LTS)

## II. Skills Demonstrated
* **Cloud Administration:** EC2 instance launch, Security Group configuration (SSH access).
* **Linux System Administration:** Command-line proficiency with `useradd`, `groupadd`, `chown`, and `chmod`.
* **Linux Security:** Implementing Discretionary Access Control (DAC) policies.
* **Security Principles:** Enforcement of the Principle of Least Privilege (PoLP).

## III. Technical Implementation Summary

### A. Directory Structure and Permissions
The core policy was enforced on the departmental shared directories (`/company_data/dev_share`, `/company_data/hr_share`).

| Access Policy | Command | Security Outcome |
| :--- | :--- | :--- |
| **Ownership** | `chown :dev /.../dev_share` | Assigns the departmental group as the owner. |
| **Permissions** | `chmod 2770 /.../dev_share` | Sets permissions to **rwx (Owner) / rws (Group) / --- (Others)**. |

### B. The Set Group ID (SGID) Bit
The leading '2' in the `chmod 2770` command sets the **Set Group ID (SGID)** bit. This is critical for shared folders because it ensures:
1.  Any file or subdirectory created inside the shared folder **automatically inherits the folder's group ownership** (`dev` or `hr`).
2.  This prevents users from creating files owned by their personal group, thereby breaking the group-level access policy.

### C. Principle of Least Privilege (PoLP) Enforcement
The "Others" permission slot was set to `0` (`---`), guaranteeing that any user who is neither the root owner nor a member of the owning group is completely **blocked** from accessing the directory.



## IV. Validation (Proof of Policy)
System validation confirmed the security policies were correctly implemented:

* **Positive Result:** Authorized users successfully created, read, and modified files in their respective departmental shares.
* **Negative Result:** Attempts by unauthorized users (e.g., an HR user attempting to access the Development share) were consistently met with a **"Permission denied"** error.

---
*The full sequence of commands used to configure the system is available in the [setup_commands.sh](setup_commands.sh) file, and a summary of the validation is logged in [validation_log.txt](validation_log.txt).*
