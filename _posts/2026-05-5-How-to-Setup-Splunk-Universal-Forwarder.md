---
title: "How to Setup Splunk Universal Forwarder"
date: 2026-05-06
categories: [SIEM]
tags: [splunk, universal-forwarder, windows-server, log-forwarding, siem, sysmon]

image:
  path: /assets/img/posts/forwarder/banner.png
  alt: Splunk Universal Forwarder Setup Guide for Windows
---
# How to Setup Splunk Universal Forwarder

---

After successfully setting up our Splunk server, the next step is to onboard endpoints and start sending logs to Splunk. In this guide, we will install and configure Splunk Universal Forwarder (UF) on a Windows machine and connect it to a Splunk server running on Ubuntu Linux.

Once configured, the forwarder will automatically send Windows Event Logs and Sysmon logs to Splunk for indexing.

---

# What is Splunk Universal Forwarder?

Splunk Universal Forwarder (UF) is a lightweight agent installed on endpoints to collect and forward logs to a Splunk Indexer. It consumes very little system resources and is commonly used to centralize logs from servers and workstations.

---

# Part 1: Configure the Splunk Server

## Enable the Receiver Port

By default, Splunk Universal Forwarder sends data using TCP port 9997. Enable the listening port on the Splunk server:

```jsx
sudo /opt/splunk/bin/splunk enable listen 9997 -auth admin:<password>
```

## Verify the Connection

After the forwarder is connected, confirm the established connection:

bash

```jsx
sudo netstat -antp | grep 9997
```

Expected output shows the indexer listening on port 9997 and an established connection from the Windows forwarder:

```jsx
tcp  0  0 0.0.0.0:9997       0.0.0.0:*              LISTEN      60475/splunkd
```

At this stage, the Splunk server is ready to receive logs from Universal Forwarders.

---

# Part 2: Install the Splunk Universal Forwarder (Windows)

Download the latest Splunk Universal Forwarder installer from the Splunk website.

Launch the installer and follow the installation wizard.

![image.png](/assets/img/posts/forwarder/image.png)

![image.png](/assets/img/posts/forwarder/image1.png)

## Accept the License Agreement

Accept the license agreement and select:

```
An on-premises Splunk Enterprise instance
```

Click **Customize Options** to continue.

![image.png](/assets/img/posts/forwarder/image2.png)

When prompted for credentials, set username to `admin` and choose a strong password.

![image.png](/assets/img/posts/forwarder/image3.png)

## Configure Receiving Indexer

Enter the hostname or IP address of your Splunk server.

Example:

```
Hostname or IP:
111.111.111.11

Port:
9997
```

The default receiving port for Splunk Universal Forwarder is 9997.

Click Next and complete the installation.

![image.png](/assets/img/posts/forwarder/image4.png)

*leave it blank

![image.png](/assets/img/posts/forwarder/image5.png)

Let the default receiving port for Splunk Universal Forwarder is 9997.

![image.png](/assets/img/posts/forwarder/image6.png)

## Verify Forwarder Installation

Once installation is complete, the Universal Forwarder should be installed in the following directory:

```jsx
C:\Program Files\SplunkUniversalForwarder
```

---

## Verify Connection from Splunk Server

Return to the Ubuntu Splunk server and verify the connection.

Run:

```jsx
sudo netstat -antp | grep 9997
```

Expected output:

```jsx
tcp       0      0 0.0.0.0:9997            0.0.0.0:*               LISTEN      60475/splunkd    
tcp        0      0 172.16.111.127:9997     172.16.111.20:59411     ESTABLISHED 60475/splunkd    
```

The ESTABLISHED state confirms that the Windows host is successfully connected to the Splunk server.

---

## Configure Log Collection

By default, Universal Forwarder does not automatically collect Sysmon logs. We need to configure the logs that should be forwarded to Splunk.

```jsx
C:\Program Files\SplunkUniversalForwarder\etc\system\local
```

Create a new file named `inputs.conf` and add the following configuration:

```jsx
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
checkpointInterval = 5
current_only = 0
disabled = 0
start_from = oldest
renderXml = true
index = sysmon

[WinEventLog://Security]
disabled = 0

[WinEventLog://Windows PowerShell]
disabled = 0
```

---

## Restart Splunk Universal Forwarder

After saving the configuration, restart the Universal Forwarder service.

Open Command Prompt as Administrator:

```
net stop splunkforwarder
net start splunkforwarder
```

Alternatively, restart the service through Windows Services.

---

# Create a Sysmon Index

Before searching for Sysmon events, we need to create a dedicated index in Splunk. This is because we configured the Universal Forwarder to send Sysmon logs to the **sysmon** index in the `inputs.conf` file.

Log in to the Splunk Web Interface and navigate to:

**Settings → Indexes**

![image.png](/assets/img/posts/forwarder/image7.png)

Scroll down until you find Indexes and click on it.

![image.png](/assets/img/posts/forwarder/image8.png)

Next, click New Index.

![image.png](/assets/img/posts/forwarder/image9.png)

Enter the index name sysmon, which matches the index specified in the `inputs.conf` configuration.

```
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = sysmon
```

Click Save to create the index.

![image.png](/assets/img/posts/forwarder/image10.png)

Once created, the new **sysmon** index should appear in the Indexes page.

![image.png](/assets/img/posts/forwarder/image11.png)

---

# Verify Log Ingestion

Log in to the Splunk Web Interface and run the following searches:

![image.png](/assets/img/posts/forwarder/image12.png)