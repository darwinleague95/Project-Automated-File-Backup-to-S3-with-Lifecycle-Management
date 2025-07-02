
# 🗂️ Automated File Backup to S3 with Lifecycle Management

## 📚 Project Overview
This project automates the backup of local files to an Amazon S3 bucket using AWS CLI and Windows Task Scheduler.  
It also applies a lifecycle policy to optimize storage costs by transitioning objects to cheaper storage after a few days and automatically deleting them after 30 days.

---

## 🛠️ Technologies Used
- Amazon S3
- AWS CLI
- Windows Task Scheduler
- Lifecycle Management Rules
- Local Backup Folder
- Logging via CMD

---

## ✅ Project Setup Steps

### 1. Create an S3 Bucket
- Bucket Name: `darwin-backup-bucket`
- Enabled: **Versioning**
- Encryption: **SSE-S3 (Amazon S3 managed keys)**

### 2. Apply Lifecycle Rule
- Transition to **Glacier Instant Retrieval** after 3 days
- Delete files automatically after 30 days

### 3. Create IAM User for CLI Access
- Type: Programmatic Access Only
- Policy: `AmazonS3FullAccess`
- Credentials: Access Key ID & Secret Access Key (configured using `aws configure`)

### 4. Configure AWS CLI
```bash
aws configure
```
Provide:
- Access Key ID
- Secret Access Key
- Region: `ap-south-1`
- Output format: `json`

### 5. Manual Backup Command
```bash
aws s3 cp "C:\Users\INTEL\OneDrive\Documents\BackupFolder" s3://darwin-backup-bucket/ --recursive
```

---

## 🔁 Automate Backup using Windows Task Scheduler

### CMD Command to Create Scheduled Task with Logging:
```bash
schtasks /create /sc daily /tn "DarwinS3Backup" /tr "cmd.exe /c aws s3 cp \"C:\Users\INTEL\OneDrive\Documents\BackupFolder\" s3://darwin-backup-bucket/ --recursive >> \"C:\Users\INTEL\OneDrive\Documents\BackupLogs\s3backup.log\" 2>&1" /st 22:00
```

### Task Summary:
- Runs daily at **910:00 PM**
- Logs all backup outputs to:
```plaintext
C:\Users\INTEL\OneDrive\Documents\BackupLogs\s3backup.log
```

---

## ✅ How to Monitor the Task
- View the log file to check for success or errors.
- Manually trigger the task using:
```bash
schtasks /run /tn "DarwinS3Backup"
```

---

## ✅ Folder Structure
```plaintext
Project_Folder/
│
├── BackupFolder/           # Local folder for backup files
├── BackupLogs/             # Folder for log files
├── s3backup.log            # Backup log file (auto-created)
├── Architecture.png        # Architecture diagram
└── README.md               # Project documentation
```

---

## ✅ Architecture Diagram

```plaintext
+--------------------------+
| Local Folder (Windows)   |
| C:\BackupFolder         |
+------------+-------------+
             |
             | AWS CLI Command
             v
+--------------------------+
| Amazon S3 Bucket         |
| darwin-backup-bucket     |
+------------+-------------+
             |
             | Lifecycle Policy
             v
+--------------------------+
| Glacier Storage          |
| (After 3 days)           |
+--------------------------+
             |
             | Auto-Delete
             v
+--------------------------+
| Permanent Deletion       |
| (After 30 days)          |
+--------------------------+
```

---

## ✅ Tools Used for Diagram
https://www.drawio.com/
You can create and export your architecture diagram as **PNG** and add it to your GitHub repository.

---

## ✅ How to Upload Files to GitHub

### Step 1: Initialize Git Repository
```bash
git init
```

### Step 2: Add Files
```bash
git add .
```

### Step 3: Commit Files
```bash
git commit -m "Added S3 backup automation project"
```

### Step 4: Connect to GitHub Repository
```bash
git remote add origin https://github.com/darwinleague95/Project-Automated-File-Backup-to-S3-with-Lifecycle-Management
```

### Step 5: Push Files
```bash
git push -u origin main
```

---

## 🎯 Summary:
- ✅ Daily automatic backup to S3
- ✅ Lifecycle cost management
- ✅ Detailed logs for each backup
- ✅ Visualized cloud architecture

---

### 👉 Feel free to contribute, report issues, or suggest improvements! 😊
