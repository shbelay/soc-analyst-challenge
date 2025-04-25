# 2 - Kibana Setup on Ubuntu Server

## SYNOPSIS

This guide documents the installation and configuration of Kibana on an Ubuntu server hosted on Vultr. The server is optimized with 400GB storage, 4 vCPUs, and 16GB memory. It is configured on a VPC network with the following public and private IP addresses:

- **Private IP**: `10.3.96.3`  
- **Public IP**: `216.128.134.227`  
- **Hostname**: `SOC-ELK-Server`

## NOTES

- **Author**: Surapheal Belay  
- **Version**: 1.0

---

## Kibana Installation

1. Visit: [https://www.elastic.co/downloads/kibana](https://www.elastic.co/downloads/kibana) and copy the link to the `.deb` package.

![image](https://github.com/user-attachments/assets/11602a7a-5cb7-460d-8130-2ddf56342342)

2. In the Ubuntu server terminal, run:

   ```bash
   wget https://artifacts.elastic.co/downloads/kibana/kibana-8.17.2-amd64.deb
   ```
![image](https://github.com/user-attachments/assets/c9eb3092-87de-40dd-b964-f7981d4ee99b)

3. Install the package:

   ```bash
   sudo dpkg -i kibana-8.17.2-amd64.deb
   ```
![image](https://github.com/user-attachments/assets/09858dd9-4142-4395-b342-7a26d43f99e4)

4. Edit the configuration file:

   ```bash
   nano /etc/kibana/kibana.yml
   ```
![image](https://github.com/user-attachments/assets/04c986b7-1416-4cd7-ac4d-2fe8ac80513b)

   - Uncomment `server.port`
   - Change `server.host` to the public IP address

![image](https://github.com/user-attachments/assets/3aac4bf6-68e6-4fe5-a98c-bdff5e9c48e1)

5. Reload, enable, and start Kibana:

   ```bash
   systemctl daemon-reload
   systemctl enable kibana.service
   systemctl start kibana.service
   ```
![image](https://github.com/user-attachments/assets/e606bcf3-2b70-46a8-a2b1-a9d4a0c31a75)

---

## Elasticsearch Integration

1. Generate an Elasticsearch token:

   ```bash
   cd /usr/share/elasticsearch/bin
   ./elasticsearch-create-enrollment-token --scope kibana
   ```
![image](https://github.com/user-attachments/assets/52494f61-9236-4ed6-ab64-add426182a7e)

2. Save the generated token.

3. Open Kibana in a browser:

   ```
   http://<public-ip>:5601
   ```

   > If it doesn’t work, allow the firewall ports:

   ```bash
   ufw allow 5601
   ufw allow 9200
   ```
![image](https://github.com/user-attachments/assets/c9524f37-28a0-4d73-ba21-bf2ca3807b30)

4. Enter the enrollment token in the Kibana GUI.

![image](https://github.com/user-attachments/assets/6ef8217d-71df-40a6-ab93-37e618e54b96)


---

## Additional Setup

### Retrieve the Kibana verification code:

```bash
cd /usr/share/kibana/bin
./kibana-verification-code
```
![image](https://github.com/user-attachments/assets/8100e558-6a6e-44c6-923a-d1d48b86a268)

### Generate encryption keys:

```bash
cd /usr/share/kibana/bin
./kibana-encryption-keys generate
```
![image](https://github.com/user-attachments/assets/0b470c94-f6a9-4170-b74c-2d9f188d3359)

### Add encryption keys to Kibana keystore:

```bash
./kibana-keystore add xpack.encryptedSavedObjects.encryptionKey
./kibana-keystore add xpack.reporting.encryptionKey
./kibana-keystore add xpack.security.encryptionKey
```
![image](https://github.com/user-attachments/assets/890332ac-494c-46ec-acde-cb7b535ce865)
![image](https://github.com/user-attachments/assets/4bac23c1-2f91-4268-843f-2b70a352ec2d)

### Restart Kibana:

```bash
systemctl restart kibana.service
```
![image](https://github.com/user-attachments/assets/a2613f12-cb96-4ad9-8f86-0bc5bd6da29f)
