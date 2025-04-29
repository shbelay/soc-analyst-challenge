# Ingest Sysmon and Windows Defender Logs into Elasticsearch

## Objective
Guide for ingesting Sysmon and Windows Defender logs into Elasticsearch.

---

## Steps

### 1. Add Integration
- On the **Elastic Home Page**, click **Add Integrations**.

![image](https://github.com/user-attachments/assets/2ed8f180-f26d-41c0-869d-52a46244d3df)

### 2. Add Custom Windows Event Logs
- Sysmon does not have a default Elastic integration.
- Click **Custom Windows Event Logs**.
- Select **Add Custom Windows Event Logs** (top right).

![image](https://github.com/user-attachments/assets/98017553-7f15-4c0b-9819-19ca0bae6b09)
![image](https://github.com/user-attachments/assets/773257a6-2abe-4ed9-85f9-b1282f853715)

### 3. Enter Integration Settings
- Fill out the **Name** and **Description**.
- For **Channel Name**, paste the full name of the Sysmon location from Event Viewer:
  - On Windows Server:
    - Open **Event Viewer**.
    - Expand **Applications and Services Logs** → **Microsoft** → **Windows** → **Sysmon**.
    - Right-click **Operational**, select **Properties**, and copy the **Full Name**.

![image](https://github.com/user-attachments/assets/6c78492a-787e-406c-8de6-7bf1ebe09533)
![image](https://github.com/user-attachments/assets/2bb6edc2-833f-4009-aa58-ea5d3fc839ca)

### 4. Select Existing Host
- In **Step 2**, choose **Existing Hosts** and select the server where the **Elastic Agent** is installed.
- Save and continue / Save and deploy changes.

![image](https://github.com/user-attachments/assets/94943bad-3ae1-4167-9ef7-7c54f97aecdc)

### 5. Add Microsoft Defender Logs
- Follow the same steps as for Sysmon:
  - Locate the Microsoft Defender logs path in Event Viewer.
  - Copy the full name.

- Collect specific Event IDs only:
  - [Event ID 1116](https://learn.microsoft.com/en-us/defender-endpoint/troubleshoot-microsoft-defender-antivirus#event-id-1116)
  - [Event ID 1117](https://learn.microsoft.com/en-us/defender-endpoint/troubleshoot-microsoft-defender-antivirus#event-id-1117)
  - [Event ID 5001](https://learn.microsoft.com/en-us/defender-endpoint/troubleshoot-microsoft-defender-antivirus#event-id-5001)

- Click **Advanced Options** and specify these **Event IDs** to be collected.

![image](https://github.com/user-attachments/assets/483e1095-88a1-48ea-b3bc-5d305167d332)
![image](https://github.com/user-attachments/assets/cd9f85d6-9fe7-4a41-9267-b496a01425e7)
![image](https://github.com/user-attachments/assets/42360228-797f-41b6-9e9d-9fc1115a7d48)

### 6. Add Agent Policies
- Attach the agent policies to the existing host.
- Save and continue / Save and deploy.

### 7. Verify Data Ingestion
- Go to the **Hamburger Menu** and click **Discover**.
- Verify that logs are successfully ingested.


---

## Notes
- Ensure **Port 9200** is open to allow data ingestion into Elasticsearch.
- Always validate that the correct Event Channel Names and Event IDs are configured.
- Periodically check ingestion status under **Discover** to ensure continuous log collection.
