# Sysmon Setup Guide

This guide walks you through the steps for setting up Sysmon on a Windows system using PowerShell.

---

## About Sysmon

[Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) is a free system utility from Microsoft that's part of the Sysinternals suite. It monitors Windows events and provides logging to detect suspicious activity on a system. Sysmon can monitor various events such as process creations, network connection initiations, and more. A configuration file is optional but highly recommended to fine-tune which events should be captured. Pre-made configuration files can be found at [Olaf Hartong's sysmon-modular repository](https://github.com/olafhartong/sysmon-modular).

--- 

## Step 1: Open PowerShell as Administrator

Ensure you run PowerShell with elevated privileges to execute the commands successfully.

---

## Step 2: Navigate to Sysmon Folder

Change the directory to where your Sysmon executable and configuration file are located:

```powershell
cd "C:\Path\To\SysmonFolder"
```
![image](https://github.com/user-attachments/assets/03eca1ac-10db-4d0c-a244-206afe364da0)

---

## Step 3: Install Sysmon

Run the following command to install Sysmon:

```powershell
.\Sysmon64.exe -i
```
![image](https://github.com/user-attachments/assets/bf990d63-f486-4900-815e-4abe8fc71bfd)
![image](https://github.com/user-attachments/assets/d7b9afd1-529c-4f27-8300-f290c1db1caa)

---

## Step 4: Confirm Sysmon is Running

You can confirm Sysmon is working properly through one of the following methods:

### Method 1: Event Viewer
Navigate to:

```
Event Viewer > Applications and Services Logs > Microsoft > Windows > Sysmon
```

Sysmon logs should appear here.

### Method 2: Services
Check if "Sysmon" is listed among Windows services:

1. Press `Win + R`, type `services.msc`, and press Enter.
2. Look for "Sysmon" in the list of services.

![image](https://github.com/user-attachments/assets/6a20dba5-c20b-4ce2-910f-a3f484512249)
---
