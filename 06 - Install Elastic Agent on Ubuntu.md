# Install Elastic Agent on Ubuntu Server

## Objective
Guide to install Elastic Agent onto a Linux Ubuntu Server.

---

## Steps

### 1. Create Agent Policy
- Go to the **Elastic Web GUI**.
- Navigate to: **Management** > **Fleet** > **Agent Policies** > **Create Agent Policy**.
- Enter the policy name: `Linux Policy`.

![image](https://github.com/user-attachments/assets/3de6ff80-2161-43d4-98c7-33efb31206d5)

### 2. Configure Integration Settings
- When configuring the integration, it will collect logs from `/var/log/auth.log` for both failed and successful login attempts.

![image](https://github.com/user-attachments/assets/4532a34a-99f4-4fbd-9f2c-71d41ff51a14)

### 3. Add Agent
- Go back to **Fleet** > **Agents**.
- Select **Add Agent** on the right-hand side.

![image](https://github.com/user-attachments/assets/1f2b9a33-d00a-4599-af72-b46679eee3e7)

### 4. Select Agent Policy
- From the drop-down menu, select the created policy: `Linux Policy`.

![image](https://github.com/user-attachments/assets/f6b3c2ed-3df8-4620-adac-3e1a16a7feef)

### 5. Enroll Agent in Fleet
- Under Step 2, select the first bullet: **Enroll in Fleet**.

![image](https://github.com/user-attachments/assets/2723ecf6-14cf-4d37-b26e-223d08b4f9fe)

### 6. Install Elastic Agent on Ubuntu Host
- Under Step 3, select the appropriate OS (Ubuntu) and copy the provided installation command.
- On your Ubuntu server:
  - Go to the home directory:
    ```bash
    cd ~
    ```
  - Paste and run the copied command.

![image](https://github.com/user-attachments/assets/0f742b7f-d402-4f9f-abea-2440403bdb4b)
![image](https://github.com/user-attachments/assets/90d147b5-fa8b-4fc0-9808-5ae9ea55b91b)

### 7. Confirm Data Ingestion
- In **Discover**, locate the field `agent.name` and select the Linux server name.
- Search for authentication failures from failed login attempt IP addresses:
  - Copy an IP from the authentication failure events.
  - In the search bar, input the IP address followed by `and authentication failure`.

![image](https://github.com/user-attachments/assets/1b186a0f-9f53-4b48-96ee-5bf1733161de)
![image](https://github.com/user-attachments/assets/52ea0d8b-1156-48a7-8587-d30ecf4fda47)
![image](https://github.com/user-attachments/assets/081abee9-4c82-4db2-abbe-ae2ffb673a3f)
![image](https://github.com/user-attachments/assets/7df71158-d12a-446c-bf44-5b0e64b6a8a9)

### 8. Customize Discover Table
- Expand one of the authentication failure events.
- Locate the `message` field.
- Hover over the field and select **Toggle column in table** to add the `message` field to the main view.

![image](https://github.com/user-attachments/assets/912c45c6-7b48-4e13-88ae-1642a4ce371e)
![image](https://github.com/user-attachments/assets/686b8469-48ac-44f3-bc76-68d04daab2ba)
![image](https://github.com/user-attachments/assets/e0ef109c-2d8b-4e42-8363-001bfba6d414)


---

## Notes
- Make sure the correct policy is selected when enrolling the agent.
- Regularly verify that data ingestion is successful and events are properly parsed.
