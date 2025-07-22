
# ✅ VPC Peering Step-by-Step Guide: N. Virginia ↔ Ohio

---

## 🌐 Step 1: Create Public VPC (N. Virginia)
- **VPC Name:** `VPC_NV`
- **CIDR Block:** e.g., `172.30.0.0/16`
- Create a **public subnet**
- Create an **Internet Gateway**
- Attach Internet Gateway to `VPC_NV`
- Update **Route Table** to allow:
  - `0.0.0.0/0` → **Internet Gateway**
- Associate the route table with the **public subnet**

---

## 🌐 Step 2: Create Private VPC (Ohio)
- **VPC Name:** `VPC_OH`
- **CIDR Block:** e.g., `192.168.0.0/16`
- Create a **private subnet**
  - Disable: **Auto-assign Public IP**
- No Internet Gateway created for this VPC

---

## 🔁 Step 3: Create VPC Peering
- **Create from N. Virginia (VPC_NV)**
  - Use Ohio VPC ID (`VPC_OH`) as **Acceptor**
- Accept the Peering Connection from **Ohio Region**

📝 **Note:** Peering is created only in **N. Virginia**, where the **Internet Gateway** exists

---

## 📡 Step 4: Route Table Updates (Cross-Region Peering)

### 🔄 In Ohio (VPC_OH):
- Go to **Private Subnet Route Table**
- Add Route:
  - Destination: `172.30.0.0/16` (CIDR of VPC_NV)
  - Target: **Peering Connection**

### 🔄 In N. Virginia (VPC_NV):
- Go to **Public Subnet Route Table**
- Add Route:
  - Destination: `192.168.0.0/16` (CIDR of VPC_OH)
  - Target: **Peering Connection**

---

## 🚀 Step 5: Launch EC2 Instances

### 🖥 Public EC2 (N. Virginia)
- **VPC:** `VPC_NV`
- **Subnet:** Public subnet
- Enable **Auto-assign Public IP**
- Update **Security Group**:
  - Inbound: SSH (22), HTTP (80), or **All TCP**
  - Outbound: All traffic (default)

### 🔒 Private EC2 (Ohio)
- **VPC:** `VPC_OH`
- **Subnet:** Private subnet
- Disable **Auto-assign Public IP**
- Update **Security Group**:
  - Inbound: Allow **SSH**, **ICMP**, **All TCP** from `VPC_NV`
  - Outbound: All traffic

---

## 🔐 Step 6: SSH from Public EC2 to Private EC2

1. Transfer `.pem` key used to create **Private EC2** into **Public EC2**
   ```bash
   vim peering.pem
   # paste key content and save
   chmod 400 peering.pem
   ```

2. Run SSH command:
   ```bash
   ssh -i peering.pem ubuntu@192.168.100.9
   ```

📌 **Example IPs:**
- Public EC2 IP: `44.214.142.248`
- Private EC2 IP: `192.168.100.9`

---

## 👤 OS Usernames Reference

| OS             | Username   |
|----------------|------------|
| Amazon Linux 2 | `ec2-user` |
| Ubuntu         | `ubuntu`   |
| RHEL           | `ec2-user` |
| CentOS         | `centos`   |

📝 Case-sensitive! Use the right name exactly.

---

## 🔐 Final Outcome

- Public EC2 in `VPC_NV` acts as a **bastion host**
- Private EC2 in `VPC_OH` is **reachable via SSH** from the public instance
- Internet is indirectly provided to the private server via **VPC Peering**

---

## ⚠️ Troubleshooting Notes
- Double-check **Security Groups**
- Make sure **route tables are cross-linked properly**
- Only **one peering connection** is needed
- **SSH connection requires** correct `.pem` key & IP
