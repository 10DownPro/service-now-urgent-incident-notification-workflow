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

## 🔧 Implementation Summary

- Edited the default "Kura Workload 1" flow
- Updated trigger conditions for category (Network) + priority (Critical 1)

<img width="1200" height="559" alt="Screenshot 2025-07-30 at 5 30 53 PM" src="https://github.com/user-attachments/assets/b2018bda-1201-4f0b-a2d6-429c860c0738" />

- Created email for Networking Operations group (networkops@yourcompany.com)<br>
- Added email action to alert the Networking group<br>

<img width="1203" height="604" alt="Screenshot 2025-07-30 at 5 31 54 PM" src="https://github.com/user-attachments/assets/b99cb0bd-2765-4adc-80da-6659b04c7d19" />

- Tested with sample data to confirm the fix

<img width="1202" height="611" alt="Screenshot 2025-07-30 at 5 34 28 PM" src="https://github.com/user-attachments/assets/c37970e0-84ba-48bd-a0c1-ff826a958c30" />

<img width="1261" height="594" alt="Screenshot 2025-07-30 at 9 25 50 PM" src="https://github.com/user-attachments/assets/bcdee7b3-b76b-40cd-aa5f-45736069f8cc" />

<img width="1261" height="596" alt="Screenshot 2025-07-30 at 9 23 15 PM" src="https://github.com/user-attachments/assets/b41df41f-bcea-4438-86da-64cab62eb503" />

## 🔍 Why I Built This

There was a real issue where a critical outage went unnoticed for hours. This workflow closes that gap so the team doesn't miss urgent problems again.

## 🧠 Bonus: AI Scenario

This project also includes ideas for how AI could make incident response even smarter by routing issues based on time zones, who's available, and who's best at solving similar problems.

---

