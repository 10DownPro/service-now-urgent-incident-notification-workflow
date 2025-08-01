# 🚨 Preventing SLA Breaches with a Critical Alert Workflow in ServiceNow

**Scenario:** A network outage hits Uber's San Francisco data center and goes unnoticed for two hours. Uber takes a takes a hit. Thousands of riders and drivers are affected, and Uber faces revenue loss and regulatory issues, all because a critical alert never reaches the right people.

This project simulates the fix. I used ServiceNow's Flow Designer to build an automated workflow that detects critical network incidents and instantly sends email alerts to the Networking Operations team. I also added a UI Policy to catch mistakes early. It warns users if they log a network issue without marking it as critical. It’s a simple but powerful solution designed to make sure engineers don’t miss what matters most, and users don't under-prioritize something that could potentially bring systems down.

---

## 📁 Project Files

/service-now-urgent-incident-notification-workflow  
├── Diagram.png  
├── README.md  
├── urgent-incident-notification-workflow.xml  

---

## ✅ What It Does

- Listens for new incidents with:
  - Category: **[Network]**
  - Priority: **[1 - Critical]**
- Sends an email alert to the **Networking Operations** group
- Prevents SLA breaches by speeding up response time
- Ensures alerts go only to on-call engineers

---

## 📊 Incident Notification Flow

This diagram shows how the workflow is triggered when a critical network incident is created and how it routes the alert to the Networking Operations team.

![Architecture Diagram](https://github.com/user-attachments/assets/d8d432b0-b979-4085-855b-ce7c258b4796)

---

## 🔧 Implementation Walkthrough

I started by reviewing the “Kura Workload 1” flow and quickly saw the issue: it wasn’t filtering for the right type of incidents. There were no conditions set for *Network* category or *Critical* priority.

I fixed that by updating the trigger so it only runs when both are true.

**🔁 Updated Trigger Conditions**  
This flow now triggers only when an incident is created with `Category = Network` and `Priority = 1 - Critical`.  
<img width="1200" height="559" alt="Trigger Conditions" src="https://github.com/user-attachments/assets/b2018bda-1201-4f0b-a2d6-429c860c0738" />

Once the flow knows an urgent incident is happening, I added an action that sends an email to the Networking Operations group.

**📨 Email Action Setup**  
The flow sends an email alert to the Networking Operations group when the conditions are met.  
**🛡️ Access Control Note:** Only engineers in the Networking Operations group receive the alert — keeping sensitive data secure and avoiding unnecessary noise.  
<img width="1203" height="604" alt="Email Action" src="https://github.com/user-attachments/assets/b99cb0bd-2765-4adc-80da-6659b04c7d19" />

Then I created a sample incident to test it all end to end.

**🧪 Test Incident Record**  
I logged a test incident with the correct category and priority to simulate a real-world outage.  
<img width="1202" height="611" alt="Test Incident" src="https://github.com/user-attachments/assets/c37970e0-84ba-48bd-a0c1-ff826a958c30" />

**📬 Email Log (sys_email Record)**  
This record confirms that the system triggered an actual email alert based on the flow.  
<img width="1261" height="594" alt="Email Log" src="https://github.com/user-attachments/assets/bcdee7b3-b76b-40cd-aa5f-45736069f8cc" />

**👥 User Group Record (Networking Operations)**  
I made sure the Networking Operations group had a working email address so alerts could be received.  
<img width="1261" height="596" alt="Group Record" src="https://github.com/user-attachments/assets/b41df41f-bcea-4438-86da-64cab62eb503" />

**✅ UI Policy: Warns Users When Network Incidents Aren’t Marked Critical**
I created a UI Policy to add an extra layer of accountability. It checks for network related incidents that aren’t marked as “Critical.” If someone sets the category to “Network” but chooses a lower priority, the system will show a warning directly on the form.

What It Does:
If the category is Network and the priority is not set to 1 - Critical, then the user will see this message:

⚠️ Network incidents are usually high priority. Confirm this is not critical.

💻 How I Built It:
**Table:** Incident

**Conditions:**

Category is Network

Priority is not 1 - Critical

Script (Run if True):

<pre> ```javascript function onCondition() { g_form.showFieldMsg( 'priority', '⚠️ Network incidents are usually high priority. Confirm this is not critical.', 'warning' ); } ``` </pre>

**1. UI Policy Conditions Setup**
<img width="1077" height="571" alt="Screenshot 2025-08-01 at 2 53 00 PM" src="https://github.com/user-attachments/assets/a2a1c452-230a-43e4-89a9-d9e401d2052e" />

**2. Script Tab with Message**
<img width="1075" height="580" alt="Screenshot 2025-08-01 at 2 53 28 PM" src="https://github.com/user-attachments/assets/4083eb63-405e-40be-854c-5078a8eea5d5" />

**3. Warning Displayed on Form**
<img width="1077" height="531" alt="Screenshot 2025-08-01 at 2 54 54 PM" src="https://github.com/user-attachments/assets/ccad7300-520b-453c-854d-8b3a0ec902eb" />


---

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

After I did that, both records appeared in the `Customer Updates` tab and were included in the final exported XML file.

**Before:**  
<img width="1041" height="344" alt="Before Fix" src="https://github.com/user-attachments/assets/d2ea15f5-1d03-4af3-8d7c-506f9a5f5ede" />

**After:**  
<img width="1202" height="591" alt="After Fix" src="https://github.com/user-attachments/assets/6fe91ad2-a7e2-403d-b987-45d1ae4c9062" />

---

## 🤖 AI Scenario: Smarter Incident Routing

Right now, incidents just trigger a standard email based on priority and categor, but AI could take that to the next level. Instead of just alerting whoever's on the list, an AI agent could route incidents based on who’s actually available, what timezone they’re in, and which engineers have handled similar issues before. Over time it could even learn who resolves issues fastest and prioritize them first. That would help cut down response time and help avoid escalation. For example, if a critical incident happens at 2AM in San Francisco, the system could automatically ping someone in a different region who's skilled and online instead of waiting for a local engineer to wake up. Basically, AI could make sure incidents go to the *right* person, not just *any* person at the best time.

---

## 💭 What I Took Away

This project reminded me that the smallest missing piece like a broken email alert can have a massive impact. I didn’t just fix a flow, I designed something that could actually prevent chaos. That’s what I love about tech.. solving problems that matter, one workflow at a time.

## Acknowledgments
Parts of the UI Policy validation script were generated with help from ChatGPT by OpenAI to help speed up development.

