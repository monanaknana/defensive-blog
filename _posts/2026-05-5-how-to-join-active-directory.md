---
title: "How To Join Windows Server to Active Directory"
date: 2026-05-5
categories: [Windows]
tags: [active-directory, windows-server]

image:
  path: /assets/img/posts/ad/banner.png
  alt: Windows Active Directory Guide
---


# How to Join Windows Server to Active Directory

---

This guide shows how to connect a **Windows machine to Active Directory (AD)**. This tutorial is for **Windows users only**. Let’s get started!

---

# STEPS

### 1. Open Network Settings

Go to:

**Control Panel → Network and Internet → Network and Sharing Center**

### 2. Open Adapter Settings

Click:

**Change adapter settings**

### 3. Choose Your Network Adapter

Select the network adapter you are using:

- Wi-Fi
- Ethernet
- Or any adapter connected to the same network

Then:

**Right-click → Properties**

![image.png](/assets/img/posts/ad/p1.png)

### 4. Configure DNS

Click:

**Internet Protocol Version 4 (TCP/IPv4)** → **Properties**

Change the **Preferred DNS Server** to your **Active Directory server IP address**.

![image.png](/assets/img/posts/ad/p2.png)

### 5. Join the Domain

Now go to:

**Server Manager → System Properties**

You will see **Computer Name** and **Member Of**.

- You can change the computer name if you want.
- Enter the domain name based on your Active Directory setup.

![image.png](/assets/img/posts/ad/p3.png)

Then click OK, and the following window will appear.

![image.png](/assets/img/posts/ad/p4.png)

That’s it 🎉

Your Windows machine should now be connected to the Active Directory domain.