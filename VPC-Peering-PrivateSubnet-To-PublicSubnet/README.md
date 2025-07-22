
# 🌐 VPC Peering: Connect Public EC2 to Private EC2 Across Regions

## 📌 Objective:
Establish a **secure connection between two VPCs in different AWS regions** (e.g., N. Virginia and Ohio) using **VPC Peering**, allowing a **private EC2 (Ohio)** to be accessed via a **public EC2 (Virginia)**.

---

## 📶 Steps and Detailed Explanation

### 1. Create VPC in N. Virginia (Requester VPC)
- **CIDR Block**: Example `172.30.0.0/16`
- This VPC will host the public EC2 and the **Internet Gateway**.

> 💡 N. Virginia is the initiator (requester) of the VPC peering request.

---

### 2. Create Subnet in N. Virginia VPC
- Use part of CIDR (e.g., `172.30.0.0/24`)
- Enable **Auto-assign Public IP**

---

### 3. Create and Attach Internet Gateway (IGW)
- Create IGW → Attach to N. Virginia VPC
- Update Route Table:
  ```
  Destination: 0.0.0.0/0 → Target: igw-xxxxxx
  ```

> 🚀 IGW is essential for public internet access to EC2 in N. Virginia

---

### 4. Create VPC in Ohio (Acceptor VPC)
- **CIDR Block**: Example `192.168.100.0/24`
- Intended for the **private subnet and EC2**.

> ⚠️ Disable "Auto-assign Public IP" in this VPC.

---

### 5. Create VPC Peering Connection
- **Initiate Peering** in N. Virginia
  - Provide **Ohio VPC ID** as acceptor
- Cross-region is supported
- Accept the peering request in **Ohio region**

> 🔁 Only **one VPC peering connection** is needed for both-way communication

---

### 6. Update Route Tables for Both VPCs

#### Ohio VPC (Private)
- Go to Route Table of **private subnet**
- Add route:
  ```
  Destination: 172.30.0.0/16 → Target: pcx-xxxxxx
  ```

#### Virginia VPC (Public)
- Go to Route Table of **public subnet**
- Add route:
  ```
  Destination: 192.168.100.0/24 → Target: pcx-xxxxxx
  ```

> 🔒 For security: Use exact CIDRs instead of `0.0.0.0/0`

---

### 7. Launch EC2 Instances

#### 🔹 Public EC2 (N. Virginia)
- Subnet: Public
- Auto-assigned Public IP ✅
- Security Group:
  - Inbound: SSH (22), HTTP, All TCP (for internal connectivity)

#### 🔹 Private EC2 (Ohio)
- Subnet: Private
- No public IP ❌
- Security Group:
  - Inbound: SSH, ICMP, and all TCP **from Public EC2's private IP or SG**

> ⚠️ Always verify SGs! AWS sometimes skips SG rules.

---

### 8. SSH from Public EC2 to Private EC2 (Peering Test)

#### Step-by-step:
1. Copy the **private EC2's key pair** content from local system
2. Paste into a file using `vim` on Public EC2:

```bash
vim peering.pem
# paste key content
chmod 400 peering.pem
```

3. Connect to private EC2 using private IP:

```bash
ssh -i peering.pem ec2-user@192.168.100.9
```

> ✅ Now you're inside the private EC2 from the public EC2

---

## 👥 Default Usernames for EC2 Access

| OS            | Username     |
|---------------|--------------|
| Amazon Linux  | ec2-user     |
| Ubuntu        | ubuntu       |
| RHEL          | ec2-user     |
| CentOS        | centos       |

> 🔑 Usernames are **case-sensitive**

---

## 🔐 Tips and Extra Notes

- VPC Peering does **not support transitive routing**
- Cannot use overlapping CIDRs between peered VPCs
- Security Groups must explicitly allow inbound from the peer VPC
- VPC Peering is regional, but **inter-region is supported**
- Consider adding **Name tags** to all components for easy identification

---

## 🧪 Summary of Outcome

| Component        | Region     | Public | Access Method                  |
|------------------|------------|--------|--------------------------------|
| Public EC2       | Virginia   | ✅     | SSH from anywhere              |
| Private EC2      | Ohio       | ❌     | SSH via Public EC2 (Peering)   |

---

## 📎 Useful CLI Commands (optional)

```bash
# Accept VPC peering via CLI (Ohio)
aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxxxx

# Add route manually
aws ec2 create-route --route-table-id rtb-xxxx --destination-cidr-block 172.30.0.0/16 --vpc-peering-connection-id pcx-xxxx
```

---

## ✅ Mission Accomplished
- 🟢 VPC Peering created and accepted across regions
- 🟢 Route tables updated on both sides
- 🟢 Public EC2 used as a bastion to reach Private EC2
