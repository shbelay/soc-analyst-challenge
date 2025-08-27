# osTicket Setup Guide on Windows Server with XAMPP

## Objective
Set up the open-source support ticketing system **osTicket** on a Windows Server using **XAMPP**, phpMyAdmin, and a public-facing configuration.

---

## 1. Initial Setup

### Spin Up Windows Server VM
- Ensure the server is publicly accessible (i.e. has a public IP address).

### Install XAMPP
- Download and install XAMPP on your Windows Server.

### Modify Apache Configuration
- Navigate to:
  ```
  C:\xampp
  ```
- Edit the properties file and update:
  ```
  apache_domainname=127.0.0.1
  ```
- Replace `127.0.0.1` with your **public IP address** and save the file.

<img width="472" height="611" alt="image" src="https://github.com/user-attachments/assets/10a2eb37-1d9c-4782-9154-51f508cace4a" />

### Open Firewall Ports
- Open **Windows Defender Firewall with Advanced Security**.
- Add a new **Inbound Rule**:
  - Type: **Port**
  - Ports: `80, 443`
  - Action: **Allow the connection**
  - Profile: Leave default
  - Name: `Inbound Connection`

<img width="837" height="123" alt="image" src="https://github.com/user-attachments/assets/f25881b4-6ebf-45c5-92ca-73f097e1e3eb" />

---

## 2. Start Apache and Configure MySQL

### Start Apache via XAMPP
- Open the **XAMPP Control Panel**.
- Start the **Apache** service.
- Click **Admin** to open phpMyAdmin.

<img width="663" height="436" alt="image" src="https://github.com/user-attachments/assets/37cf900c-6cbb-4a5b-afe1-7041af9eef04" />

### Configure User in phpMyAdmin
1. Go to **User Accounts** → **Edit privileges** for `localhost`.
2. Click **Login Information** tab.
3. Set:
   - Hostname → **Use text field** → Enter **public IP**
   - Set a **password**
4. Click **Go** to apply changes.

### Edit phpMyAdmin Config File
- Open:
  ```
  C:\xampp\phpMyAdmin\config.inc.php
  ```
- Update these sections:
  - **Bind IP** → replace with your public IP.
  - **Advanced features user** → add the password created above.

<img width="505" height="287" alt="image" src="https://github.com/user-attachments/assets/094a528f-feec-48b2-9c15-8dfebda89092" />

---

## 3. Create Database for osTicket

1. In phpMyAdmin, click **New** to create a database (e.g., `osticket_db`).
2. Go to **User Accounts**.
3. Find the `root` user with your public IP and click **Edit privileges**.
4. Under the **Database** tab:
   - Select the new database.
   - Check **All privileges**.
   - Click **Go** to save.

---

## 4. Install osTicket

### Download and Extract osTicket
- Download osTicket from the [official website](https://osticket.com/download).
- Navigate to:
  ```
  C:\xampp\htdocs
  ```
- Create a new folder named:
  ```
  osticket
  ```
- Extract the downloaded package.
- Copy contents from `upload/` and `scripts/` folders into the new `osticket` directory.

### Run Installer
- Open browser and navigate to:
  ```
  http://<public-ip>/osticket/upload/setup/
  ```
- Complete installation by following on-screen prompts.
- Click **Install Now** at the end.

### Final URLs
- **osTicket Portal**:
  ```
  http://<public-ip>/osticket/upload/
  ```
- **Staff Control Panel**:
  ```
  http://<public-ip>/osticket/upload/scp
  ```

---

## Notes
- Ensure Apache and MySQL services are always running in XAMPP.
- Use a strong password for MySQL user accounts.
- Test inbound firewall access externally to confirm public visibility.
- Secure the server post-installation to limit unauthorized access.
