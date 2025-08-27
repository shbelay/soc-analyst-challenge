# osTicket + ELK Integration Guide

## Objective
Integrate **osTicket** with the **Elastic Stack (ELK)** using a webhook connector to enable alert-based ticket creation.

---

## 1. Prepare osTicket for API Integration

### Access the Staff Control Panel
- Open your browser and go to:
  ```
  http://45.32.204.30/osticket/upload/scp
  ```

### Create an API Key
1. Log into the **Admin Panel**.
2. Go to **Manage** ➝ **API** ➝ **Add New API Key**.
3. Enter the IP address of the **ELK server**.

<img width="546" height="570" alt="image" src="https://github.com/user-attachments/assets/8471597f-74f5-424f-a853-ca9b40915ef2" />

4. Click **Add Key**.

An API key will be generated. Example:
```
BFBAC65F5099F85CA2B74EFA33AD165C
```

You will use this API key in the Elastic webhook integration.

---

## 2. Configure Webhook in Elastic

### Access the Elastic Portal
1. Navigate to:
   - **Management** ➝ **Stack Management** ➝ **Alerts and Insights** ➝ **Connectors**

### Create Webhook Connector
1. Click **Create Connector**.
2. Select **Webhook** as the connector type.
3. Configure the following:
   - **Connector Name**: `osTicket Webhook`
   - **Method**: `POST`
   - **URL**:
     ```
     http://45.32.204.30/osticket/upload/api/tickets.xml
     ```
   - **Authentication**: `None`
   - **Enable HTTP Header**:
     - **Key**: e.g., `X-API-Key`
     - **Value**: Paste the generated API key from osTicket

Click **Save** to complete the connector setup.

---

## Notes
- Ensure ports `80/443` are open between Elastic and the osTicket server.
- The payload format sent by Elastic should match the expected structure in `tickets.xml` API.
- Secure the API key and limit its use to the Elastic server only.
- You may test the webhook connector by triggering a test alert in Kibana.
