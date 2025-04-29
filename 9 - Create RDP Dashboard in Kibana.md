# Create RDP Dashboard in Kibana

## Objective
Create a dashboard in Kibana to visualize RDP activity using event logs and maps.

---

## Steps

### 1. Open Kibana Maps
- Go to the **Elastic Home Page**.
- Navigate to **Maps**.

### 2. Write Query for Failed RDP Logins
- Enter the following query and set the appropriate time frame:
  ```
  event.code:4625 and agent.name: "Windows-Server-2022"
  ```
- Click **Add Layer** and choose **Choropleth**.

![image](https://github.com/user-attachments/assets/a8a1f8b3-c0ed-470c-bc19-7d420d7c3666)

### 3. Save Map Visualization
- Once the map displays data, click **Add and Continue**.
- Save the map at the top of the window.

![image](https://github.com/user-attachments/assets/81e00b49-b69b-4798-9760-b9ca92a37b28)

### 4. Save Search for Successful RDP Logins
- Use the following query:
  ```
  event.code:4624 and (winlog.event_data.LogonType : 10 or winlog.event_data.LogonType : 7)
  ```
- Save it as:
  ```
  RDP successful activity
  ```
![image](https://github.com/user-attachments/assets/0f3d998b-e5df-4588-a3c2-a8ac6bf99e0b)

### 5. Create a Table Visualization
- In the dashboard, click **Create Visualization**.
- To enhance the dashboard, add a table that displays:
  - `user.name`
  - `source.ip`
- This complements the map for quicker insights.

### 6. Add SSH Reference Visualization (Optional)
- Copy the following query and use it in the visualization tool if combining with Linux SSH logs:
  ```
  system.auth.ssh.event : * and agent.name: "Linux-Server" and system.auth.ssh.event: "Failed"
  ```

---

## Notes
- Choropleth maps are useful for visualizing geolocation-based login patterns.
- Adding a table helps correlate usernames and IPs involved in RDP activity.
- Grouping visualizations improves operational efficiency when monitoring RDP access.
