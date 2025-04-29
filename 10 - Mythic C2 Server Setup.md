# Mythic Server Setup for Command and Control (C2)

## Overview
Command and Control (C2) is where an attacker sends instructions and receives updates from compromised systems. 
After breaching a network, attackers use a C2 channel to maintain access, move laterally, or steal data stealthily. 
This guide covers setting up a **Mythic server** to simulate C2 operations.

---

## Installation Steps

### 1. Install Required Packages
```bash
apt install docker-compose
apt install make
```

### 2. Clone Mythic Repository
```bash
git clone https://github.com/its-a-feature/Mythic
```

### 3. Setup Mythic Server
```bash
cd Mythic
./install_docker_ubuntu.sh
./mythic-cli start
```

### 4. Configure Firewall
- Restrict firewall access to only allow connections from:
  - Your computer
  - Targeted machines (RDP and SSH connections)

![image](https://github.com/user-attachments/assets/c0afa3bd-a0df-414e-94e8-c7c668522fa9)

### 5. Access Mythic Server GUI
- Open a browser and navigate to:
  ```
  https://<mythic-server-ip>:7443
  ```
  Example:
  ```
  https://144.202.70.131:7443/
  ```

### 6. Retrieve Login Credentials
- On the Mythic server, go to the Mythic directory.
- View the `.env` file:
  ```bash
  cat .env
  ```
- Find:
  - **Username** under `MYTHIC_ADMIN_USER`
  - **Password** under `MYTHIC_ADMIN_PASSWORD`

Example credentials:
```bash
MYTHIC_ADMIN_USER="mythic_admin"
MYTHIC_ADMIN_PASSWORD="sbVzJBl7eHwllGrTDa8wL5SAxbSY0I"
```

- Use these credentials to log in to the Mythic GUI.

![image](https://github.com/user-attachments/assets/4375a88e-3b20-4af8-ac5f-aa993412f645)
![image](https://github.com/user-attachments/assets/9bf9bfec-cf26-438a-a946-4f892c1ca441)
![image](https://github.com/user-attachments/assets/db581107-72e8-4a9a-9d94-fc233eacf413)

---
