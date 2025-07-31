# ⚡️ Critical Incident Alerts in ServiceNow (Built with Flow Designer)

This is a ServiceNow workflow I built to automatically send email alerts when a critical network incident happens. It helps make sure the right team (Network Operations) is notified right away so they can jump in and fix the issue fast.

## 📁 Project Files

/service-now-urgent-incident-notification-workflow  
├── README.md  
├── urgent-incident-notification-workflow.xml  
├── Diagram.png  

## ✅ What It Does

- Listens for new incidents with:
  - Category: **Network**
  - Priority: **1 - Critical**
- Sends an email alert to the **Network Operations** group
- Helps prevent SLA breaches by speeding up response time

## 📊 Incident Notification Flow

This diagram shows how the workflow is triggered when a critical network incident is created, and how it routes the alert to the Network Operations team.

![Architecture Diagram](https://github.com/user-attachments/assets/d8d432b0-b979-4085-855b-ce7c258b4796)

## 🔧 Implementation Summary

- Edited the default "Kura Workload 1" flow
- Updated trigger conditions for category (Network) + priority (Critical 1)

**🔁 Updated Trigger Conditions**  
This flow triggers only when an incident is created with `Category = Network` and `Priority = 1 - Critical`.  
<img width="1200" height="559" alt="Trigger Conditions" src="https://github.com/user-attachments/assets/b2018bda-1201-4f0b-a2d6-429c860c0738" />

**📨 Email Action Setup**  
The flow sends an email alert to the Networking Operations group when the conditions are met.  
<img width="1203" height="604" alt="Email Action" src="https://github.com/user-attachments/assets/b99cb0bd-2765-4adc-80da-6659b04c7d19" />

**🧪 Test Incident Record**  
I created a sample incident to simulate a real-world network outage.  
<img width="1202" height="611" alt="Test Incident" src="https://github.com/user-attachments/assets/c37970e0-84ba-48bd-a0c1-ff826a958c30" />

**📬 Email Log (sys_email Record)**  
This record confirms the email notification was successfully generated and sent.  
<img width="1261" height="594" alt="Email Log" src="https://github.com/user-attachments/assets/bcdee7b3-b76b-40cd-aa5f-45736069f8cc" />

**👥 User Group Record (Networking Operations)**  
The Networking Operations group was updated with a valid email address to receive alerts.  
<img width="1261" height="596" alt="Group Record" src="https://github.com/user-attachments/assets/b41df41f-bcea-4438-86da-64cab62eb503" />

## ⚠️ Troubleshooting: What I Ran Into

### 1. Test Records Not Added to Update Set

When I added my test incident and email records, they didn’t show up in the `Customer Updates` tab of my update set. I realized they had been added to the **Default** update set instead of the one I created.

**Why it happened:**  
I forgot to set my custom update set (`Kura Workload 1`) as **Current** before adding those records.

**How I fixed it:**
- Went to `Local Update Sets`
- Opened `Kura Workload 1` and saw it was already marked **Complete**, which prevented me from making it current
- Changed the **State** back to `In Progress`
- Then clicked **Set as Current**
- Re-added the test incident and email record using **“Add to Update Set”** on both records

Once I did that, both records appeared in the `Customer Updates` tab and were included in the final exported XML file.

**Before:**  
<img width="1041" height="344" alt="Before Fix" src="https://github.com/user-attachments/assets/d2ea15f5-1d03-4af3-8d7c-506f9a5f5ede" />

**After:**  
<img width="1202" height="591" alt="After Fix" src="https://github.com/user-attachments/assets/6fe91ad2-a7e2-403d-b987-45d1ae4c9062" />

## 🤖 AI Scenario: Smarter Incident Routing

Right now, incidents just trigger a standard email based on priority and category — but AI could take that to the next level. Instead of just alerting whoever's on the list, an AI agent could route incidents based on who’s actually available, what timezone they’re in, and which engineers have handled similar issues before. It could even learn over time which people resolve issues fastest and prioritize them first. That would cut down response time big time and help avoid escalation. For example, if a critical incident happens at 2AM in San Francisco, the system could automatically ping someone in a different region who's skilled and online instead of waiting for a local engineer to wake up. Basically, AI could make sure incidents go to the *right* person, not just *any* person at the best time.


---
