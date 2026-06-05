---
title: "How to Install and Set Up Splunk Enterprise on Linux"
date: 2026-05-05
categories: [SIEM]
tags: [splunk, siem, ubuntu, linux, soc, monitoring, log-analysis]

image:
  path: /assets/img/posts/splunk/splunk.png
  alt: Splunk Enterprise Installation Guide on Ubuntu Linux
---

# How to Install and Set Up Splunk Enterprise on Linux

---

Before we start collecting and analyzing logs, we first need to set up our Splunk server. Splunk is a powerful platform that allows us to collect, monitor, and search logs from different systems in one place.

In this guide, I will walk you through the installation and basic configuration of Splunk Enterprise on an Ubuntu Linux server. Don't worry if this is your first time using Splunk, just follow the steps and by the end of this tutorial you should have a fully working Splunk instance ready for log monitoring and analysis.

---

# Lab Environment

This tutorial uses the following environment:

- Ubuntu Server 24.04 LTS – Splunk Enterprise Server
- Internet Connection – Required for downloading packages
- Splunk Enterprise 10.4.0
- Web Browser – To access the Splunk Web Interface

The Ubuntu server will act as the centralized log management platform where all logs will be collected, indexed, and analyzed.

---

# Prerequisites

Before getting started, make sure you have:

- A Linux server (Ubuntu/Debian-based)
- Internet access
- A Splunk account
- Sudo privileges

---

## Step 1: Create a Splunk Account

Visit the official Splunk website and sign up for a free account.

After registration, Splunk will send a verification email to your inbox. Open the email and click **Verify Your Email** to activate your account.

[Splunk website](https://www.splunk.com/)

You should receive an email similar to this:

- Subject: **Activate Account**
- Click **Verify Your Email**
- Complete the account activation process

![image.png](/assets/img/posts/splunk/image.png)

![image.png](/assets/img/posts/splunk/image1.png)

---

## Step 2: Download Splunk Enterprise

After logging in to the Splunk website, navigate to the Splunk Enterprise download page and select the Linux (.deb) installation package.

Splunk provides a wget command that allows the package to be downloaded directly from the terminal. The download URL may vary depending on the Splunk version selected.

```jsx
wget -O splunk-10.4.0-f798d4d49089-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.4.0/linux/splunk-10.4.0-f798d4d49089-linux-amd64.deb"
```

![image.png](/assets/img/posts/splunk/image2.png)

Verify that the file has been downloaded successfully:

```jsx
splunkadmin@splunk-usim:~$ ls -lh
total 1.3G
-rw-rw-r-- 1 splunkadmin splunkadmin 1.3G May 18 17:57 splunk-10.4.0-f798d4d49089-linux-amd64.deb
```

---

## Step 3: Install Splunk

Install the downloaded package using `dpkg`:

```jsx
sudo dpkg -i splunk-10.4.0-f798d4d49089-linux-amd64.deb
```

```jsx
Selecting previously unselected package splunk.
(Reading database ... 88417 files and directories currently installed.)
Preparing to unpack splunk-10.4.0-f798d4d49089-linux-amd64.deb ...
verify that this sytem has all the commands we will require to perform the preflight step
no need to run the splunk-preinstall upgrade check
Unpacking splunk (10.4.0) ...
Setting up splunk (10.4.0) ...
find: ‘/opt/splunk/lib/python3.7/site-packages’: No such file or directory
complete
```

The installation process may take a few minutes to complete. Once finished, Splunk will be installed under:

```jsx
/opt/splunk
```

Once the package has been downloaded successfully, install it using the dpkg package manager. During the installation process, Splunk files will be extracted and installed under the /opt/splunk directory.

---

## Step 4: Start Splunk for the First Time

Start Splunk and accept the license agreement:

```jsx
sudo /opt/splunk/bin/splunk start --accept-license --run-as-root
```

```jsx
This appears to be your first time running this version of Splunk.

Splunk software must create an administrator account during startup. Otherwise, you cannot log in.
Create credentials for the administrator account.
Characters do not appear on the screen when you type in credentials.
```

During the initial startup, Splunk requires an administrator account to be created. This account will be used to access the Splunk Web Interface and perform administrative tasks such as data onboarding, index management, and application installation.

### Password Requirements

Your password must contain:

- At least 8 characters
- A combination of letters, numbers, and special characters

```jsx
Please enter an administrator username: admin
Password must contain at least:
   * 8 total printable ASCII character(s).
Please enter a new password:
Please confirm new password:
```

---

## Step 5: Wait for Initialization

During startup, Splunk will:

- Generate SSL certificates
- Create required directories
- Validate indexes
- Check system configurations
- Start Splunk services

You will see messages such as:

```jsx
Copying '/opt/splunk/etc/openldap/ldap.conf.default' to '/opt/splunk/etc/openldap/ldap.conf'.
writing RSA key

writing RSA key

Moving '/opt/splunk/share/splunk/search_mrsparkle/modules.new' to '/opt/splunk/share/splunk/search_mrsparkle/modules'.

Splunk> Winning the War on Error

Checking prerequisites...
        Checking http port [8000]: open
        Checking mgmt port [8089]: open
        Checking appserver port [127.0.0.1:8065]: open
        Checking kvstore port [8191]: open
        Checking configuration... Done.
                Creating: /opt/splunk/var/lib/splunk
                Creating: /opt/splunk/var/run/splunk/appserver/i18n
                Creating: /opt/splunk/var/run/splunk/appserver/modules/static/css
                Creating: /opt/splunk/var/run/splunk/upload
                Creating: /opt/splunk/var/run/splunk/search_telemetry
                Creating: /opt/splunk/var/run/splunk/search_log
                Creating: /opt/splunk/var/spool/splunk
                Creating: /opt/splunk/var/spool/dirmoncache
                Creating: /opt/splunk/var/lib/splunk/authDb
                Creating: /opt/splunk/var/lib/splunk/hashDb
                Creating: /opt/splunk/var/run/splunk/collect
                Creating: /opt/splunk/var/run/splunk/sessions
New certs have been generated in '/opt/splunk/etc/auth'.
New certs have been generated in '/opt/splunk/etc/auth'.
        Checking critical directories...        Done
        Checking indexes...
                Validated: _audit _configtracker _dm_summary _dsappevent _dsclient _dsphonehome _internal _introspection _metrics _metrics_rollup _telemetry _thefishbucket history main summary
        Done
        Checking filesystem compatibility...  Done
        Checking conf files for problems...
        Done
        Checking default conf files for edits...
        Validating installed files against hashes from '/opt/splunk/splunk-10.4.0-f798d4d49089-linux-amd64-manifest'
        All installed files intact.
        Done
All preliminary checks passed.

Starting splunk server daemon (splunkd)...
Using configuration from /opt/splunk/share/openssl3/openssl.cnf
..+.........+......+..+...+....+.....+....+.....+............+............+.............+..+.+++++++++++++++++++++++++++++++++++++++*......+++++++++++++++++++++++++++++++++++++++*.....+.+..+.......+.....+....+.........+...+..+......+.+........+......+......+....+...........+.........+.+...+...+.....+.........+.+...+..+.......+...........+....+.....+.+..............+......+.+......+..+......+.......+.........+......+...+.....+..........+..+....+...........+...+.+........+......+.+.........+......+........+......+.......+........+.+...+..+.+.....+...+....+.....+..........+......+............+..............+.+...+.....+.+...........+....+.....+...+...++++++
..........+++++++++++++++++++++++++++++++++++++++*..+.+++++++++++++++++++++++++++++++++++++++*...............+..+......+...+......+..........+........+...+.......+...+..+.+...........+.+.................+......+....+..+...+..........+......+..+...+......+.+...+.................+....+........+.+..+...+.+...+.....+.......+...........+..................+.+...+..+....+.....+....+..+....+...........+......+...+....+.....+.+......+........+...............+.+............+..+.......+.....+.............+...+...+....................+.........+.+..+.............+.....+.+.....+..................+......+.....................+.+...+..+............+.+......+......+.........+.....+....+............+..+......+.......+..+.......+.........+..............+...+....+.....+....+......+...+........+.........+..........+..............+.+......+............+..+......+.........+.............+.....+......+.+...+...............+......+......+.....+...+.+..+...+....+..+..........+...........+.......+...+..+..........++++++
Warning: ignoring -extensions option without -extfile
Certificate request self-signature ok
subject=CN=splunk-usim, O=SplunkUser
Done

Waiting for web server at http://127.0.0.1:8000 to be available............... Done
```

*This process may take several minutes.

## Step 6: Access the Splunk Web Interface

Once the startup process is complete, Splunk will display a message:

```jsx
If you get stuck, we're here to help.
Look for answers here: http://docs.splunk.com

The Splunk web interface is at http://name:8000
```

After all initialization tasks have completed successfully, Splunk Web will become available through port 8000. Open a web browser and navigate to the below:

```jsx
http://<server-ip>:8000
```

Log in using the administrator account you created earlier.

![image.png](/assets/img/posts/splunk/image3.png)

---

## Conclusion

Congratulations! You have successfully installed and configured Splunk Enterprise on Ubuntu Linux.