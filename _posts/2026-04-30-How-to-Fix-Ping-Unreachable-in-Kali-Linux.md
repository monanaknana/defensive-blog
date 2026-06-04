---
title: "How to solve if ping is unreachable in Linux"
date: 2026-04-30
categories: [Linux]
tags: [vmware, networking]

image:
  path: /assets/img/posts/ping/ping.png
  alt: Fix ping unreachable in Kali Linux

pin: true
toc: true
comments: true
---

# How to Fix Ping Unreachable in Kali Linux (VMware Workstation)

I had a problem with ping being unreachable in my Kali Linux VM. After looking 
online, I could not find any written guide to solve this — most solutions were 
video-only — so I decided to write this guide myself.

### Prerequisites:
- VMware Workstation installed on Windows
- Kali Linux running as a guest VM
- An active network adapter on your host machine

### Why does this happen?
The issue occurs because VMware's Bridged network adapter is set to Automatic 
by default, which may not correctly detect your active network adapter, causing 
the ping to be unreachable.

## Steps

### Step 1: Open the VMware Workstation installation folder

Navigate to the following path in Windows Explorer:

```plaintext
C:\Program Files (x86)\VMware\VMware Workstation
```

### Step 2: Run vmnetcfg.exe as Administrator

Look for the file named `vmnetcfg.exe` in that folder:

![VMware installation directory showing vmnetcfg.exe](/assets/img/posts/ping/step.png)

Right-click on `vmnetcfg.exe` and select **Run as administrator**:

![Right-click context menu showing Run as administrator option](/assets/img/posts/ping/step1.png)

### Step 3: Locate the Bridged network adapter 

Once the Virtual Network Editor opens, you will see that the external connection 
for the Bridged type is set to **Automatic** by default:

![Virtual Network Editor showing Bridged adapter set to Automatic](/assets/img/posts/ping/step2.png)

### Step 4: Change the adapter to your active network adapter

Change the Bridged adapter from **Automatic** to your own active network adapter. 
Select the adapter that matches your current connection — for example, 
`Intel(R) Wi-Fi 6 AX203`. Your adapter name may be different depending on 
your machine:

![Virtual Network Editor showing adapter changed to Intel Wi-Fi](/assets/img/posts/ping/step3.png)

### Step 5: Apply the changes

Click **Apply** and then **OK** to save the settings.

### Step 6: Restart your VM network and verify

After applying the changes, restart your Kali Linux VM or reconnect the network 
adapter. You should now be able to ping successfully:

![Terminal showing successful ping result](/assets/img/posts/ping/step4.png)

## Troubleshooting

If the issue persists after following the steps above, try the following:

- Make sure your host machine is connected to the internet before starting the VM
- Try selecting a different network adapter from the dropdown
- Restart VMware Workstation entirely and repeat the steps
- Ensure your VMware Workstation is up to date

> **Note:** Steps may vary slightly depending on your version.