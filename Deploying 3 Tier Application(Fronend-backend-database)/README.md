
# 🚀 Deploying a 3-Tier Web Application (Frontend + Backend + Database)

### Architecture Overview

- **Frontend**: React.js (Hosted on EC2 + Apache)
- **Backend**: Java Spring Boot API (Hosted on EC2)
- **Database**: MariaDB (AWS RDS)

---

## 🧰 Technologies Used

| Layer      | Technology         |
|------------|--------------------|
| Frontend   | React.js           |
| Backend    | Java, Maven        |
| Database   | MariaDB (AWS RDS)  |
| Hosting    | AWS EC2 (Ubuntu)   |
| Code Repo  | GitHub             |
| Terminal   | MobaXterm (SSH GUI)|

---

## 📦 Project Structure

- `cloudblitz-student-app`: Full-stack project (frontend + backend)
- `EasyCRUD`: Helper repository for setup steps

---

## 1️⃣ RDS: Setting Up MariaDB

1. Open AWS RDS and create a MariaDB database.
2. Set the DB name, username, and password.
3. Choose **"Connect to EC2"** (if you want private access).
4. Configure **Security Group**:
   - Open ports: `3306` (MySQL), etc.
5. Wait for the database status to become **Available**.
6. Copy the RDS **endpoint URL** for use in backend connection.

---

## 2️⃣ EC2: Launch a Virtual Machine

1. Launch an **Ubuntu** instance.
2. Use instance type: `t2.micro` (free tier).
3. Configure Security Group to allow:
   - `22` (SSH), `80` (HTTP), `443` (HTTPS)
4. Download the **.pem SSH key**.

---

## 🔑 Access EC2 via MobaXterm

1. Download [MobaXterm](https://mobaxterm.mobatek.net/)
2. Open **SSH Terminal**.
3. Fill in:
   - Remote Host = EC2 Public IP
   - Username = `ubuntu` (for Ubuntu)
   - SSH Key (.pem)
4. Click **OK** to connect.

---

## 🔄 Install Backend Dependencies

1. Become root:
   ```bash
   sudo su
   ```

2. Clone backend repo:
   ```bash
   git clone https://github.com/ganrajdol99215/cloudblitz-student-app.git
   cd cloudblitz-student-app/backend
   ```

3. Install Java, Maven, JDK:
   ```bash
   sudo apt update
   sudo apt install openjdk-11-jdk maven -y
   ```

4. Edit `application.properties` with RDS DB credentials:
   ```bash
   vim src/main/resources/application.properties
   ```

5. Build backend:
   ```bash
   mvn clean package
   ```

6. Run backend:
   ```bash
   java -jar target/student-registration-backend-0.0.1-SNAPSHOT.jar
   ```

   To run in background:
   ```bash
   nohup java -jar target/student-registration-backend-0.0.1-SNAPSHOT.jar > backend.log 2>&1 &
   ```

---

## 🌐 Frontend Setup (React.js)

1. Clone repo:
   ```bash
   cd ~
   git clone https://github.com/ganrajdol99215/EasyCRUD.git
   cd EasyCRUD/frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   npm i axios web-vitals
   ```

3. Configure `.env`:
   ```
   REACT_APP_API_BASE_URL=http://<EC2_PUBLIC_IP>:<API_PORT>
   ```

4. Build project:
   ```bash
   npm run build
   ```

---

## 🌍 Host Frontend on Apache

1. Install Apache:
   ```bash
   sudo apt install apache2 -y
   ```

2. Deploy build:
   ```bash
   sudo cp -rf dist/* /var/www/html/
   ```

3. Start Apache:
   ```bash
   sudo systemctl start apache2
   sudo systemctl status apache2
   ```

---

## ✅ Verify Everything

- **Access App**: Open your EC2 Public IP in browser.
- **Register Data**: Use the app UI to register entries.
- **Verify Database**:
   ```bash
   mysql -u admin -p -h <RDS_ENDPOINT>
   ```

---

## 🔗 Useful Links

- [Backend Repo](https://github.com/ganrajdol99215/cloudblitz-student-app)
- [Helper Repo (EasyCRUD)](https://github.com/ganrajdol99215/EasyCRUD)

---

## 🏁 Summary

This 3-Tier architecture uses:
- **RDS** for DB layer,
- **EC2** for hosting React and Java backend,
- **GitHub** for version control,
- **Apache** to serve the React build.

> ✅ You have now deployed a scalable, cloud-native 3-tier web application.
