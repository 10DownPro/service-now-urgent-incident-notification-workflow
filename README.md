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
  - Category: **Network**
  - Priority: **1 - Critical**
- Sends an email alert to the **Networking Operations** group
- Prevents SLA breaches by speeding up response time
- Ensures alerts go only to on-call engineers

---

## 💢 What Was Broken (Before the Fix)

This is what the original flow looked like before I made any changes. It wasn't filtering for critical network incidents, so no alerts were getting triggered when they should’ve.

| View | Screenshot |
|------|------------|
| ❌ Broken Email Node | ![Broken Trigger](https://github.com/user-attachments/assets/53c008a5-6b06-419f-8a41-66354b0acca3) |
| ❌ Broken Trigger (No Filtering) | ![Broken Email](https://github.com/user-attachments/assets/f2472024-15ee-4f21-bf03-8b728f46393a) |

---

## 📊 Incident Notification Flow

This diagram shows how the workflow is triggered when a critical network incident is created and how it routes the alert to the Networking Operations team.

![Architecture Diagram](https://github.com/user-attachments/assets/d8d432b0-b979-4085-855b-ce7c258b4796)

---

## 🔧 Implementation Walkthrough

I started by reviewing the original “Kura Workload 1” flow and saw the problem immediately: no conditions were set to filter for *network* and *critical* incidents. That meant every incident got ignored by the flow.

So I updated the trigger logic. Now the flow only runs when:
- `Category = Network`
- `Priority = 1 - Critical`

Then I added an email action to alert the Networking Operations group. I also double-checked access control to make sure the email only goes to that group and not everyone in the org.

After building the flow, I created a sample test incident with the correct category and priority. Once submitted, I confirmed that an email was triggered and logged.

To catch user-side mistakes, I also added a **UI Policy**. If someone tries to submit a network incident with a lower priority than critical, they’ll see a warning right on the form.

---

## 📸 Screenshots

| View | Screenshot |
|------|------------|
| ✅ Trigger Conditions | ![Trigger](https://github.com/user-attachments/assets/b2018bda-1201-4f0b-a2d6-429c860c0738) |
| ✉️ Email Action Setup | ![Email Node](https://github.com/user-attachments/assets/b99cb0bd-2765-4adc-80da-6659b04c7d19) |
| 🧪 Test Incident Record | ![Test](https://github.com/user-attachments/assets/c37970e0-84ba-48bd-a0c1-ff826a958c30) |
| 📨 Email Log | ![Log](https://github.com/user-attachments/assets/bcdee7b3-b76b-40cd-aa5f-45736069f8cc) |
| 👥 Group Record | ![Group](https://github.com/user-attachments/assets/b41df41f-bcea-4438-86da-64cab62eb503) |

## 🔁 Before vs After: Workflow Comparison

| State | View | Screenshot |
|-------|------|------------|
| ❌ **Before** | Broken Trigger Conditions | ![Broken Email](https://github.com/user-attachments/assets/f2472024-15ee-4f21-bf03-8b728f46393a) |
| ✅ **After** | Fixed Trigger Conditions | ![Trigger](https://github.com/user-attachments/assets/b2018bda-1201-4f0b-a2d6-429c860c0738) |
| ❌ **Before** | Broken Email Node | ![Broken Trigger](https://github.com/user-attachments/assets/53c008a5-6b06-419f-8a41-66354b0acca3) |
| ✅ **After** | Fixed Email Node | ![Email Node](https://github.com/user-attachments/assets/b99cb0bd-2765-4adc-80da-6659b04c7d19) |


---

## ⚠️ UI Policy Validation

I created a UI Policy to prevent under-prioritizing network incidents.

If the category is set to **Network** and priority is not **1 - Critical**, it shows this message:

> ⚠️ Network incidents are usually high priority. Confirm this is not critical.

### UI Policy Script

```javascript
function onCondition() {
  g_form.showFieldMsg(
    'priority',
    '⚠️ Network incidents are usually high priority. Confirm this is not critical.',
    'warning'
  );
}
```

| View | Screenshot |
|------|------------|
| ⚙️ **UI Policy Conditions Setup**<br><sub>Runs when asset type = Laptop</sub> | ![UI Policy Conditions](https://github.com/user-attachments/assets/a2a1c452-230a-43e4-89a9-d9e401d2052e) |
| 💬 **Script Tab with Message**<br><sub>Warns if serial number has letters</sub> | ![Script Tab](https://github.com/user-attachments/assets/4083eb63-405e-40be-854c-5078a8eea5d5) |
| 🖥️ **Warning Displayed on Form**<br><sub>Shows validation message on bad input</sub> | ![Form Warning](https://github.com/user-attachments/assets/ccad7300-520b-453c-854d-8b3a0ec902eb) |

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

| State | Screenshot |
|-------|------------|
| ❌ **Before** | <img width="1041" height="344" alt="Before Fix" src="https://github.com/user-attachments/assets/d2ea15f5-1d03-4af3-8d7c-506f9a5f5ede" /> |
| ✅ **After** | <img width="1202" height="591" alt="After Fix" src="https://github.com/user-attachments/assets/6fe91ad2-a7e2-403d-b987-45d1ae4c9062" /> |


---

## 🤖 AI Scenario: Smarter Incident Routing

Right now, incidents just trigger a standard email based on priority and categor, but AI could take that to the next level. Instead of just alerting whoever's on the list, an AI agent could route incidents based on who’s actually available, what timezone they’re in, and which engineers have handled similar issues before. Over time it could even learn who resolves issues fastest and prioritize them first. That would help cut down response time and help avoid escalation. For example, if a critical incident happens at 2AM in San Francisco, the system could automatically ping someone in a different region who's skilled and online instead of waiting for a local engineer to wake up. Basically, AI could make sure incidents go to the *right* person, not just *any* person at the best time.

---

## 💭 What I Took Away

This project reminded me that the smallest missing piece like a broken email alert can have a massive impact. I didn’t just fix a flow, I designed something that could actually prevent chaos. That’s what I love about tech.. solving problems that matter, one workflow at a time.

## Acknowledgments
Parts of the UI Policy validation script and the screenshot table formatting were created with help from ChatGPT by OpenAI. This assistance helped speed up development and improve the clarity of documentation.
