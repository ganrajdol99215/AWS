
# NAT Gateway: Public Server Internet to Private Server (via VPC)

## 🧩 Use Case
We want to give internet access to a **private EC2 instance (no public IP)** via a **public EC2 server** using a **NAT Gateway**, without directly exposing the private server to the internet.

---

## 📶 Step-by-Step Summary

### 1. Create a Custom VPC
- **CIDR Block**: `192.168.0.0/16`
- Purpose: Isolate from default VPC; gain full control over subnets and routing.

---

### 2. Create Two Subnets
| Type | CIDR | Auto-Assign Public IP |
|------|------|------------------------|
| Public Subnet | `192.168.0.0/17` | **Enabled** ✅ |
| Private Subnet | `192.168.128.0/18` | **Disabled** ❌ |

> Both can be in the same availability zone.

---

### 3. Set Subnet IP Assignment
- Go to **Subnet settings → Edit** → Uncheck “Auto-assign public IPv4” for private subnet.
- Enable for public subnet.

---

### 4. Route Table Behavior
- VPC auto-generates a route table per subnet.
- No **Internet Gateway (IGW)** by default in custom VPCs.
- IGW must be created and attached manually.

---

### 5. Create and Attach Internet Gateway (IGW)
- Create IGW, attach to your custom VPC.
- Update the public subnet's route table:

```
Destination: 0.0.0.0/0 → Target: igw-xxxxxxxx
```

---

### 6. Create NAT Gateway
- Must be deployed in the **public subnet**.
- Requires an **Elastic IP (EIP)**.
- Allows:
  - ✅ Outbound access for private servers
  - ❌ No inbound access from internet

---

### 7. Create Route Table for Private Subnet
- Associate only with private subnet.
- Add NAT Gateway in the route:

```
Destination: 0.0.0.0/0 → Target: nat-xxxxxxxx
```

---

### 8. Launch EC2 Instances

#### Public EC2 Instance
- Subnet: Public
- Auto-assign IP: Yes
- Security Group:
  - SSH (22) from your IP
  - HTTP/HTTPS as needed

#### Private EC2 Instance
- Subnet: Private
- Auto-assign IP: No
- Uses NAT for internet access

---

### 9. Access Private Server via Public EC2 (Bastion Method)

#### Tools Used: MobaXterm
1. SSH into public EC2 using `.pem` key
2. Upload private EC2 key to public server
3. Run:

```bash
chmod 400 Linux.pem
ssh -i 'Linux.pem' ec2-user@<Private-EC2-Private-IP>
```

---

## 🧠 Key Concepts

| Concept | Description |
|---------|-------------|
| **NAT Gateway** | Allows outbound internet for private subnets. Must be in public subnet. |
| **Bastion Host** | Public EC2 used to access private EC2s. |
| **IGW (Internet Gateway)** | Needed for public subnet internet access. |
| **Elastic IP** | Static public IP for NAT gateway. |
| **Route Table** | Defines routing via IGW or NAT per subnet. |
| **Security Group** | Manages instance access (SSH, HTTP, etc). |

---

## ✅ Final Outcome

- ✅ Public EC2: Has internet + SSH
- ✅ Private EC2: Internet via NAT, no direct public access
- ✅ SSH into private only through public (bastion/jump host)

---

## 🛠 Optional Enhancements
- Diagram of architecture (VPC, Subnets, IGW, NAT, EC2)
- Terraform code to automate setup
- Script to verify instance internet access

