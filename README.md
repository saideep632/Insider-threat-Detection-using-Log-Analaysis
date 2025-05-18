Got it! You want a clean, focused section for **Ubuntu and Windows configuration** related to your *Insider Threat Detection Using Log Analysis* project. Here’s a concise, clear setup guide you can include in your project documentation or report.

---

# Insider Threat Detection Using Log Analysis

### Ubuntu and Windows Configuration

---

## Ubuntu Configuration for Log Collection

### 1. Installing Splunk Universal Forwarder on Ubuntu

* Update packages:

  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
* Download Splunk Universal Forwarder (replace version with latest):

  ```bash
  wget -O splunkforwarder-8.x.x-linux-amd64.deb 'https://download.splunk.com/products/universalforwarder/releases/8.x.x/linux/splunkforwarder-8.x.x-linux-amd64.deb'
  ```
* Install the forwarder:

  ```bash
  sudo dpkg -i splunkforwarder-8.x.x-linux-amd64.deb
  ```
* Start Splunk Universal Forwarder:

  ```bash
  sudo /opt/splunkforwarder/bin/splunk start --accept-license
  ```
* Enable boot-start:

  ```bash
  sudo /opt/splunkforwarder/bin/splunk enable boot-start
  ```
* Configure forwarder to send logs to Splunk server:

  ```bash
  sudo /opt/splunkforwarder/bin/splunk add forward-server <SPLUNK_SERVER_IP>:9997
  ```
* Add log files to monitor (e.g., syslog):

  ```bash
  sudo vi /opt/splunkforwarder/etc/system/local/inputs.conf
  ```

  Add:

  ```
  [monitor:///var/log/syslog]
  disabled = false
  ```
* Restart forwarder:

  ```bash
  sudo /opt/splunkforwarder/bin/splunk restart
  ```

### 2. Configuring Syslog for Enhanced Log Data

* Edit `/etc/rsyslog.conf` or `/etc/rsyslog.d/50-default.conf` to ensure important logs (auth, syslog, kernel) are captured.
* Restart rsyslog service:

  ```bash
  sudo systemctl restart rsyslog
  ```

---

## Windows Configuration for Log Collection

### 1. Installing Splunk Universal Forwarder on Windows

* Download the latest Universal Forwarder MSI installer from [Splunk official site](https://www.splunk.com/en_us/download/universal-forwarder.html).
* Run the installer and follow the GUI steps.
* During installation:

  * Specify the Splunk server’s IP and port (usually `9997`).
  * Set the Universal Forwarder to run as a service.

### 2. Configuring Splunk Universal Forwarder on Windows

* Open PowerShell as Administrator.
* Navigate to Splunk Universal Forwarder bin directory, e.g.:

  ```powershell
  cd "C:\Program Files\SplunkUniversalForwarder\bin"
  ```
* Add inputs for key Windows logs (Security, System, Application):

  ```powershell
  .\splunk add monitor "C:\Windows\System32\winevt\Logs\Security.evtx"
  .\splunk add monitor "C:\Windows\System32\winevt\Logs\System.evtx"
  .\splunk add monitor "C:\Windows\System32\winevt\Logs\Application.evtx"
  ```
* Restart Splunk forwarder service:

  ```powershell
  Restart-Service SplunkForwarder
  ```


  ```powershell
  .\splunk add monitor "C:\Windows\System32\winevt\Logs\Microsoft-Windows-Sysmon%4Operational.evtx"
  ```

---

**Note:** Replace `<SPLUNK_SERVER_IP>` and ports as per your network setup.

---

