# 1 - Elasticsearch Setup on Ubuntu Server

## SYNOPSIS

This guide documents the installation and configuration of Elasticsearch on an Ubuntu server hosted on Vultr. The server is optimized with 400GB storage, 4 vCPUs, and 16GB memory. It is configured on a VPC network with the following public and private IP addresses:

- **Private IP**: `10.3.96.3`  
- **Public IP**: `216.128.134.227`  
- **Hostname**: `SOC-ELK-Server`

## NOTES

- **Author**: Surapheal Belay  
- **Date**: 2025-03-25  
- **Version**: 1.0

---

## Elasticsearch Installation

1. Visit: [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch) and copy the link to the `.deb` package.

![image](https://github.com/user-attachments/assets/0daf7bbc-0945-49c8-8616-c48ae29137a2)


2. In the Ubuntu server terminal, run the following command to download the package:

   ```bash
   wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.17.2-amd64.deb
   ```

![image](https://github.com/user-attachments/assets/a2026540-0f78-48c6-8eb9-727d50f96dd5)


3. Install the package using:

   ```bash
   sudo dpkg -i elasticsearch-8.17.2-amd64.deb
   ```

![image](https://github.com/user-attachments/assets/1ed3276e-42ab-431d-ba05-e27e58f5df1d)


4. Copy the generated “Security Autoconfiguration Information” and save it to a file. This includes the generated password for logging into Elasticsearch and instructions for resetting it.

![image](https://github.com/user-attachments/assets/5e846ef2-1a4a-474c-9c5a-93b2dff193bc)


5. Change directory to the Elasticsearch directory:

   ```bash
   cd /etc/elasticsearch
   ```

![image](https://github.com/user-attachments/assets/1cbf8756-9a71-4746-8a6f-8978b1ebcdef)


6. Open the configuration file:

   ```bash
   nano elasticsearch.yml
   ```

![image](https://github.com/user-attachments/assets/46686069-c985-42db-bb3f-d903f50c4e7f)

![image](https://github.com/user-attachments/assets/e6da9028-2d77-4c17-bcfb-ad721d15fc4c)

7. In `elasticsearch.yml`:
   - Modify the host IP: remove the comment (`#`) and input the server's IP address.
   - Uncomment the `http.port` line and ensure the correct port is set.
   - This enables SOC analysts to connect to the instance using the public IP and defined port.

![image](https://github.com/user-attachments/assets/ef7f85df-9389-44c5-9b2f-2a5dcac2c27b)


8. Reload, enable, and start the Elasticsearch service:

   ```bash
   systemctl daemon-reload
   systemctl enable elasticsearch.service
   systemctl start elasticsearch.service
   ```
![image](https://github.com/user-attachments/assets/6de72c82-6a05-4cdc-8276-6ebbbc3c703c)
