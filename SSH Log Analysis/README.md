# **Splunk Basics – SSH Log Analysis**

---

##  **Objective**

The purpose of this lab is to understand how SSH logs can be analyzed using Splunk for security monitoring.

In this lab, you will:

* Learn how to upload and process SSH logs in Splunk
* Analyze login attempts and authentication results
* Identify failed login attempts and successful connections
* Detect suspicious activity such as brute-force attacks
* Understand SSH behavior patterns from log data

---

##  **Lab Setup**

### **Requirements**

* Splunk installed and accessible via Splunk Web
* Zeek SSH logs in JSON format
* Basic understanding of SPL (Search Processing Language)

---

### **Data Source**

* **Log Type:** Zeek SSH Logs
* **Format:** JSON
* **File Name:** ssh_logs

These logs contain detailed information about SSH connections and authentication attempts.

---

## ⚙️ **Steps to Upload SSH Logs into Splunk**

1. Open Splunk Web in your browser

2. Navigate to:
   **Settings → Add Data**

3. Select the **Upload** option

4. Choose the file:
   **ssh_logs**

5. Configure input settings:

   * **Source Type:** json
     (or create a custom sourcetype like ssh_logs)

   * **Index:**

     * Use default index (**main**)
     * or create a new index (**ssh_lab**)

6. Complete the upload process

7. Verify indexing by running:

```
index=ssh_lab
```

If events are visible, the data has been successfully ingested.

---

##  **Understanding SSH Logs**

SSH logs record login attempts and connection activities between clients and servers.

Each log entry includes key details such as:

* **id.orig_h** – Source IP (client attempting connection)
* **id.resp_h** – Destination IP (server)
* **auth_success** – Indicates login success or failure
* **event_type** – Type of SSH event (successful, failed, etc.)
* **timestamp** – Time of the event

These fields help in tracking authentication behavior and identifying suspicious login activity.

---

## 🔍 **Lab Tasks**

---

### ✅ **Task 1: Identify Top IPs with Failed Login Attempts**

This task helps detect systems attempting multiple failed logins.

```
index=ssh_lab sourcetype="json" auth_success=false
| stats count by id.orig_h
| sort -count
| head 10
```

### **Explanation**

* Filters only failed login attempts
* Counts number of failures per IP
* Displays top 10 IPs with highest failures

### **Analysis**

A high number of failed attempts from a single IP may indicate a **brute-force attack**, where an attacker is trying multiple passwords.

---

### ✅ **Task 2: Count Total SSH Connections**

This task provides an overview of total SSH activity.

```
index=ssh_lab sourcetype="json"
| stats count as total_ssh_connections
```

### **Explanation**

* Counts all SSH events (successful + failed)
* Renames output as total connections

### **Analysis**

This helps understand normal traffic levels and detect abnormal spikes in SSH activity.

---

### ✅ **Task 3: Analyze SSH Event Types**

This task helps in understanding different types of SSH activities.

```
index=ssh_lab sourcetype="json"
| stats count by event_type
```

### **Explanation**

* Groups events based on type (successful, failed, etc.)

### **Analysis**

Typical observations may include:

* High **failed** events → possible attack attempts
* Unexpected **successful** logins → potential unauthorized access
* **multiple-failed / no-auth events** → automated scanning activity

---

##  **Security Insights**

SSH log analysis can help identify several security risks:

* **Brute-force attacks** – Repeated failed login attempts from same IP
* **Unauthorized access** – Logins from unknown or unusual sources
* **Suspicious timing** – Logins at abnormal hours
* **Unusual patterns** – Sudden spikes in login failures

Monitoring these patterns helps in early threat detection.

---

##  **Skills Practiced**

* Log ingestion in Splunk
* Understanding SSH log structure
* Writing SPL queries
* Analyzing authentication behavior
* Detecting suspicious login activity

---

##  **Conclusion**

This lab demonstrated how SSH logs can be analyzed using Splunk for security monitoring.

* SSH logs were successfully uploaded and indexed
* SPL queries were used to analyze login attempts and activity patterns
* Failed login attempts and suspicious behaviors were identified

This type of analysis is important for detecting attacks, monitoring access, and improving system security.

---


