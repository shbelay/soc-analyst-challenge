# 3 - Elastic Agent and Fleet Server Setup

## SYNOPSIS

At this point of the challenge, the SOC-ELK-Server is set up with Elasticsearch and Kibana on an Ubuntu server. Additional servers include:

- **Windows Server 2022**
  - Public IP: `137.220.49.153`
  - Hostname: `Windows-Server-2022`
- **Ubuntu Server**
  - Public IP: `144.202.79.165`
  - Hostname: `Linux-Server`
- **Fleet Server**
  - Public IP: `45.76.236.253`
  - Hostname: `Fleet-Server`

## DESCRIPTION

Elastic Agents are installed on the Windows and Ubuntu servers for monitoring logs, metrics, and more. These agents use policies that define integrations and endpoint protection settings. Policies determine which logs the endpoint forwards to Elasticsearch or Logstash.

Elastic Agents can be installed in two ways:

- **Standalone installation** (independent)
- **Fleet-managed installation** (centralized control)

Fleet Server manages Elastic Agents through a centralized Fleet UI, streamlining updates and integrations.

> For more on Elastic Agents:  
> [Elastic Agent Guide](https://www.elastic.co/guide/en/fleet/current/beats-agent-comparison.html#additional-capabilities-beats-and-agent)

## NOTES

- **Author**: Surapheal Belay   
- **Version**: 1.0

---

## Install Elastic Agent on Windows Server and Enroll into Fleet

1. Open Kibana at: `http://216.128.134.227:5601/`

2. Navigate to **Management > Fleet**

![image](https://github.com/user-attachments/assets/3a7c3c00-5b78-44d6-9358-fccb151c1ef3)

3. Click **Add Fleet Server**

![image](https://github.com/user-attachments/assets/53cde7bb-f062-4b5b-af72-94567a95f0b0)

4. Under **Quick Start**:
   - Enter the name and public IP of the Fleet Server
   - Use `https://<Fleet-IP>` (uses port 8220)

![image](https://github.com/user-attachments/assets/4d3cdfee-23d3-4e74-8827-194a588ff253)

5. Click **Generate Fleet Server Policy**

6. For installing Fleet Server on the Ubuntu Fleet server, run the following:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.17.2-linux-x86_64.tar.gz
   tar xzvf elastic-agent-8.17.2-linux-x86_64.tar.gz
   cd elastic-agent-8.17.2-linux-x86_64

   sudo ./elastic-agent install \
     --fleet-server-es=https://216.128.134.227:9200 \
     --fleet-server-service-token=AAEAAWVsYXNRaWMvZmxlZXQtc2VydmVyL3Rva2VuLT E3NDAwNjg3MTgyMDA6VkNabHLmQ25UNTZqTDgxRF9Nd1VkUQ \
     --fleet-server-policy=fleet-server-policy \
     --fleet-server-es-ca-trusted-fingerprint=b920f78fb35525a570463f57a95c27b87b2a2a92d13b529b06a401691e40a8dc \
     --fleet-server-port=8220
   ```

![image](https://github.com/user-attachments/assets/8b78d571-8ed3-494a-967e-dbe3a595326c)
![image](https://github.com/user-attachments/assets/74184bd0-905c-4246-b098-d25309887913)

7. On the ELK Server:
   - Allow Fleet IP in firewall
   - On the Fleet Server:
     ```bash
     ufw allow 9200
     ```

---

## Add Agent to Windows Server

1. Click **Add agent** and **Create policy**

![image](https://github.com/user-attachments/assets/5c08afb6-5f3d-4b2e-bd0a-baec05529e8b)

2. Choose **Windows** OS and run the provided command in PowerShell:

   ```powershell
   $ProgressPreference = 'SilentlyContinue'
   Invoke-WebRequest -Uri https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.17.2-windows-x86_64.zip -OutFile elastic-agent-8.17.2-windows-x86_64.zip
   Expand-Archive .\elastic-agent-8.17.2-windows-x86_64.zip -DestinationPath .
   cd elastic-agent-8.17.2-windows-x86_64
   .\elastic-agent.exe install --url=https://45.76.236.253:443 --enrollment-token=RmtaRkpKVUJhQkwxUk1VZ0hqUzc6aTY2czhLUXhRWEtUMjBxQXNkQ2NsZw==
   ```
![image](https://github.com/user-attachments/assets/3923e093-1812-41c1-8200-c854213b4f8b)

---

## Troubleshooting

- If agents fail to connect:
  - Ensure port `8220` is open:
    ```bash
    ufw allow 8220
    ```
  - In Kibana:
    - Go to **Fleet > Settings** and verify port `8220` is configured
