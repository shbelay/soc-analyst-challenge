# Create Mythic C2 Alerts and Dashboards in Kibana

## Objective
Set up custom alerts and dashboards in Kibana to monitor Mythic C2 activity via Elasticsearch, specifically targeting suspicious process creation and system behavior.

---

## Step 1: Investigate Suspicious Activity
- Run a query in **Discover**:
  ```
  svchost-belaypayload.exe and event.code : "1"
  ```
  > **Note**: Event Code 1 corresponds to **process creation** in Windows.

<img width="1606" height="402" alt="image" src="https://github.com/user-attachments/assets/cc1f631c-53f0-42f0-9924-b7a5c0e04cd9" />

- Expand the event and copy the SHA1 hash from the `winlog.event_data.Hashes` field.

<img width="472" height="122" alt="image" src="https://github.com/user-attachments/assets/202bb897-69eb-4cbf-ab86-72cd6773b370" />

- Verify the hash on [VirusTotal](https://www.virustotal.com).

<img width="473" height="287" alt="image" src="https://github.com/user-attachments/assets/4a334d85-6081-42cf-a05e-6587b072aa5c" />

Example field values to copy:
```text
winlog.event_data.OriginalFileName: Apollo.exe
winlog.event_data.Hashes: SHA1=88EEDF40215AF5CDADD411E9661A7920240B27E5
```

---

## Step 2: Create Custom Detection Rule
### Build Custom Query:
```
(event.code : "1") and (winlog.event_data.Hashes : "SHA1=88EEDF40215AF5CDADD411E9661A7920240B27E5" or winlog.event_data.OriginalFileName : "Apollo.exe")
```

### Save the Query Search
- Click **Save** in Discover and give the query a recognizable name.

<img width="641" height="617" alt="image" src="https://github.com/user-attachments/assets/9f813cac-baa8-44ce-a6c3-d21628db87a1" />

### Create the Rule
- Navigate to **Security > Rules > Detection Rules (SIEM)**.
- Click **Create New Rule**.
- Choose **Custom Query** and paste in the above query.

#### Required Fields to Map:
- `host.name`
- `message`
- `winlog.event_data.CommandLine`
- `winlog.event_data.ParentCommandLine`
- `winlog.event_data.Image`
- `winlog.event_data.ParentImage`
- `winlog.event_data.ProcessGuid`
- `winlog.event_data.User`
- `winlog.event_data.CurrentDirectory`

<img width="882" height="616" alt="image" src="https://github.com/user-attachments/assets/0c794385-4fd5-42d2-83ca-241f6f23f6a5" />

### Rule Metadata:
- Enter a name, description, and select the **severity level**.
- Keep the **schedule and rule actions** as default.
- Click **Create and Enable** to activate the rule.

<img width="497" height="398" alt="image" src="https://github.com/user-attachments/assets/304ede30-a11b-4740-a4bd-02cba186826b" />

---

## Step 3: Create Supporting Dashboards

### Event ID: 3 – Outbound Network Connections
Query:
```
(event.code : "3") and (event.provider : "Microsoft-Windows-Sysmon") and (winlog.event_data.Initiated : true) and not (winlog.event_data.Image : *MsMpEng.exe)
```

### Event ID: 1 – Suspicious Process Creations
Query:
```
(event.code : "1") and (event.provider : "Microsoft-Windows-Sysmon") and (powershell or cmd or rundll32)
```

### Event ID: 5001 – Windows Defender Disabled
Query:
```
(event.code : "5001") and (event.provider : "Microsoft-Windows-Windows Defender")
```

---

## Step 4: Build the Dashboard
- Navigate to **Analytics > Dashboards**.
- Click **Create New Dashboard**.
- For each saved query above:
  - Click **Create Visualization**.
  - Paste the corresponding query into the query bar.
  - Add the following fields to the visualization:
    - `@timestamp`
    - `host.name`
    - `message`
    - `winlog.event_data.CommandLine`
    - `winlog.event_data.ParentCommandLine`
    - `winlog.event_data.Image`
    - `winlog.event_data.ParentImage`
    - `winlog.event_data.ProcessGuid`
    - `winlog.event_data.User`
    - `winlog.event_data.CurrentDirectory`
  - Switch the display type from **bar** to **table** using the drop-down on the right.
  - Reorder the columns for readability.
  - Click **Save and Return**.

<img width="421" height="561" alt="image" src="https://github.com/user-attachments/assets/dc85e4e6-a73f-44c4-959b-23bd33c160a7" />
<img width="433" height="326" alt="image" src="https://github.com/user-attachments/assets/fa1d9378-d48a-4650-924d-34883914fa48" />
<img width="402" height="328" alt="image" src="https://github.com/user-attachments/assets/c2d9df9c-901e-428a-8503-ec7c4eb566f5" />


- Repeat this process for each event code query.
- Save the entire dashboard.

---

## Summary
This guide enables analysts to:
- Detect and alert on malicious activity tied to Mythic agents.
- Visualize critical process and network behavior.
- Monitor endpoints for Defender tampering and unusual command-line activity.
