# Create Alerts and Dashboards in Kibana

## Objective
Create an SSH brute force alert and dashboard in Kibana.

---

## Steps

### 1. Analyze SSH Brute Force Attempts
- Look for the following information:
  - **Failed login attempts**
  - **User name**
  - **Source IP address**

### 2. Discover Failed SSH Attempts
- In the **Elastic Web GUI**, click the **hamburger menu** and select **Discover**.
- Identify brute force attempts:
  - In less than 24 hours, there could be over 2,000 login attempts.

![image](https://github.com/user-attachments/assets/6e18c31d-ecb2-43e6-9f48-f26720be8e05)
![image](https://github.com/user-attachments/assets/f7236645-5006-462d-941a-3b8ddaa97122)
![image](https://github.com/user-attachments/assets/7ed981a8-a46a-4983-ac89-ddc38c2d3ee0)

### 3. Add Fields for Analysis
- On the left-hand side in Discover, click on **user.name**, **source.ip**, and **source.geo.country_name** to add it as a visible field.
- Click **Save** in the top left to save your view.

![image](https://github.com/user-attachments/assets/72480d52-cd34-4d44-a8bc-407fe11c2457)
![image](https://github.com/user-attachments/assets/90a14327-f23e-4f52-bce7-a595b211cf43)
![image](https://github.com/user-attachments/assets/fd0240ea-d0dc-4379-9fe1-93183e3ecc34)
![image](https://github.com/user-attachments/assets/89e14d3a-1d83-4055-8c4d-728b1f896aef)

### 4. Create Alerts
- In the top menu, select **Alerts**.
- Configure the alert conditions based on detected failed SSH login attempts.

![image](https://github.com/user-attachments/assets/a39f7554-a883-4a6e-9979-9f3eb49be874)

### 5. Create Dashboards
- Click the **hamburger menu**, then go to **Analytics** ➔ **Maps**.
- Enter the relevant query and click **Update**.
- Click **Add Layer** and select **Choropleth** to visualize geographic data such as source IP locations.

![image](https://github.com/user-attachments/assets/84c02bea-6cdf-486b-9d78-342bda36ded6)
![image](https://github.com/user-attachments/assets/a3f5a107-d6bc-4829-ad67-f98d1e845fb9)
![image](https://github.com/user-attachments/assets/3dcd0e55-e310-4574-a19c-a63f28fee20b)
![image](https://github.com/user-attachments/assets/6bc15dbb-c8d0-4844-91c1-9f288d996061)
![image](https://github.com/user-attachments/assets/c44c62aa-96ea-4732-839f-8a2fbd3caf9b)

---

## Notes
- Ensure you save your searches and visualizations to reuse them in dashboards.
- You can further fine-tune alert thresholds based on specific behaviors or source patterns.
