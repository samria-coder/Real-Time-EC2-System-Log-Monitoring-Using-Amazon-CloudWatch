<img width="1156" height="525" alt="log-details" src="https://github.com/user-attachments/assets/f98dd998-3a76-4a40-b983-3569ef61e99f" />
# EC2 Linux Log Monitoring with Amazon CloudWatch Agent

PROJECT DESCRIPTION

This project demonstrates how to collect system logs from an Amazon EC2 instance, send them to Amazon CloudWatch Logs, and monitor them in real time.
I configured the CloudWatch Agent manually, created a custom log file, and set up a log group & stream to visualize logs live in the AWS console.

This project shows my understanding of:

Linux file system
Log creation & management
CloudWatch log ingestion
IAM roles & permissions
Cloud monitoring & troubleshooting

ARCHITECTURE OVERVIEW

EC2 Instance → CloudWatch Agent → CloudWatch Log Group → Log Stream → Live Monitoring

STEP-BY-STEP PROCESS (Clear & Simple)
1. Launch EC2 instance (Amazon Linux 2023)
Created an EC2 instance
Attached an IAM role with CloudWatch permissions
Connected through SSH

2. Install CloudWatch Agent

Downloaded and installed the CloudWatch agent on EC2:
sudo yum install amazon-cloudwatch-agent -y

3. Create a Custom Log File

Since Amazon Linux 2023 does not have /var/log/messages, I created my own log file:
sudo journalctl -u sshd.service -f > /var/log/sshd.log &
This file now receives live SSH logs.

4. Create CloudWatch Agent Configuration File

Created the JSON configuration file:
sudo nano /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
Pasted this:

{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/sshd.log",
            "log_group_name": "EC2-SSHD-Logs",
            "log_stream_name": "sshd-log-stream"
          }
        ]
      }
    }
  }
}

This tells CloudWatch Agent:

which file to read
which log group to send logs to
which log stream to use

5. Start the CloudWatch Agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
-a fetch-config \
-m ec2 \
-c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json \
-s

6. View Logs in CloudWatch
Opened AWS console:
CloudWatch → Logs → Log Groups → EC2-SSHD-Logs → sshd-log-stream

I was able to see:

real-time SSH login events
timestamps
system messages

7. Upload Screenshots & Code to GitHub
The GitHub repo contains:
CloudWatch agent configuration file
Screenshots of log monitoring
Step-by-step documentation

WHAT THIS PROJECT DEMONSTRATES
✔ Understanding of Linux logging system
✔ Ability to install and configure CloudWatch Agent
✔ Experience with IAM roles and permissions
✔ Cloud monitoring & troubleshooting
✔ Real-world log ingestion and observation

SKILLS USED
Linux Administration
AWS EC2
AWS IAM
AWS CloudWatch Logs
JSON Configuration
Troubleshooting System Logs

HOW TO RUN THIS PROJECT (For Interviewer)
Anyone can reproduce this by:

Launch EC2
Create IAM role for CloudWatch
Install CloudWatch agent
Create any log file
Configure agent JSON
Start agent
Monitor logs in CloudWatch
