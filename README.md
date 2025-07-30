# 🚨 Urgent Incident Notification Workflow – ServiceNow

This is a ServiceNow workflow I built to automatically send email alerts when a critical network incident happens. It helps make sure the right team (Network Operations) is notified right away so they can jump in and fix the issue fast.

## 📁 Project Files

/service-now-urgent-incident-notification-workflow<br>
├── README.md # This file<br>
├── urgent-incident-notification-workflow.xml # Update set export<br>
├── Diagram.png # Visual of the notification flow<br>


## ✅ What It Does

- Listens for new incidents with:
  - Category: **Network**
  - Priority: **1 - Critical**
- Sends an email alert to the **Network Operations** group
- Helps prevent SLA breaches by speeding up response time

## 🔍 Why I Built This

There was a real issue where a critical outage went unnoticed for hours. This workflow closes that gap so the team doesn't miss urgent problems again.

## 🧠 Bonus: AI Scenario

This project also includes ideas for how AI could make incident response even smarter by routing issues based on time zones, who's available, and who's best at solving similar problems.

---

