# Create RDP Alerts in Kibana

## Objective
Observe authentication logs from a Windows Server and create alerts and dashboards for RDP activity.

---

## Steps

### 1. Access Discover View
- Go to the **Elastic Web GUI**.
- Click the **hamburger menu** and select **Discover**.

### 2. Filter for Windows Server Logs
- Filter results to show logs where:
  ```
  agent.name == windows server
  ```
- Further filter by **Event Code 4625** which indicates a failed RDP login attempt.

![image](https://github.com/user-attachments/assets/729a78dd-e845-4558-b2fa-bcfbf0808754)
![image](https://github.com/user-attachments/assets/5359a3b5-ad15-4ec7-8c53-db046fb0edd7)

### 3. Add Key Fields to View
- Add the following fields to the columns:
  - `user.name`
  - `source.ip`

![image](https://github.com/user-attachments/assets/f007aa2b-95bb-48e1-ad92-bc1413d7f9b7)

### 4. Save the Search
- Save the search with the name:
  ```
  RDP failed activity
  ```

![image](https://github.com/user-attachments/assets/5ad7afae-f538-4695-8a1c-a346a5ee73a0)

### 5. Create Detection Rule
- Go to: **Security** ➔ **Rules** ➔ **Detection Rules (SIEM)** ➔ **Create New Rule**.
- Select **Threshold Rule**.
- Configure the threshold and any other criteria needed.

![image](https://github.com/user-attachments/assets/64385398-4049-4502-abea-396c0cfe5acb)
![image](https://github.com/user-attachments/assets/802c14c7-a5eb-4b14-b5ad-8a25c8e3183a)

### 6. Finalize and Enable Rule
- Click **Continue** through the setup steps.
- Click **Create and Enable Rule** to activate it.

![image](https://github.com/user-attachments/assets/a454194a-a19f-44f6-9e3a-680da7560ae1)
![image](https://github.com/user-attachments/assets/3089a4c0-af39-4bd5-b535-ecca701e6b98)

---

## Notes
- You can tune the threshold rule to trigger based on the number of failed RDP attempts from the same IP or against the same user.
- Use the saved search in visualizations or dashboards for better analysis and reporting.
