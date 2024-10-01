# Scheduula™ - Custom Email Scheduler


**Scheduula™** is an affordable email marketing tool that gives you the freedom to connect with your audience your way. It helps manage contacts, schedule emails, and track progress efficiently. This README explains the project's features, the underlying technology, and the workflow, including a documented **Make.com** integration.


## Features


- **No restrictive platform policies:** Full control over your email campaigns.
- **Contact management:** Store contacts easily in Airtable and segment them via tags.
- **Track Metrics:** Measure open rates, clicks, sender reputation, etc.
- **No Contact Limitations:** Scale with unlimited contacts and list sizes.
- **Flexible Mail Servers:** Use AWS SES, Gmail, Sendgrid, and more.
- **Support for HTML & Plain Text Emails:** Design visually appealing emails.
- **API Integrations:** Connect with other tools to streamline workflows.
- **Sharable Forms:** Easily collect contacts via embeddable forms on websites.


## Technologies Used


- **Airtable** - Contact Management.
- **Make.com** - Workflow automation.
- **Sendgrid** - Email delivery engine.
- **Webhook** - Handling unsubscribe and other requests.
- **APIs** - Integrating external tools and services.


---


## How Scheduula Works


### 1. **Database and Contact Management:**
   - Contacts are stored in **Airtable**.
   - **Tags** are used to create **segments** to send targeted emails. 
   - Segments are created by adding tags to contacts in Airtable. The contacts can be filtered based on tags to send relevant email sequences.


### 2. **Mail Server Configuration:**
   - Scheduula can use **AWS SES**, **Sendgrid**, **Mailgun**, or even **custom SMTP servers**.
   - The current version is configured with **Sendgrid** for flexible and reliable delivery.
   
### 3. **Email Scheduling:**
   - Each contact is assigned a due date for emails in a sequence (e.g., Email 1 due on 01/01/2024, Email 2 due 2 hours after Email 1).
   - New emails in the sequence require new fields in Airtable with corresponding due dates.
   - Contacts will receive different emails in the sequence depending on when they joined the list.


### 4. **Progress Tracking:**
   - The **progress** field records the last email sent to each contact.
   - This ensures that the right email is sent to the right contact at the right time.
   - A **Make** scenario is responsible for updating this field after each email is sent.


### 5. **Unsubscription:**
   - Each user gets a unique unsubscription link.
   - Clicking this link triggers a **webhook** that pushes data back to Scheduula, updating the **unsubscribe** field in Airtable.
   - Scheduula will filter out unsubscribed contacts when sending future emails.


---


## Workflow Overview


Here is the **Make.com** workflow that powers the email scheduling system:


### **Step-by-Step Workflow Documentation:**


1. **Trigger Webhook (Module 51)**
   - This workflow starts with a **webhook trigger** from Make.com.
   - The webhook listens for contact actions (e.g., a new contact added or email interaction).
   - Configuration:
     - **Hook**: Custom webhook URL.
     - **Maximum results**: 1 (to limit results to a single contact).


2. **Fetch Contact from Airtable (Module 52)**
   - After receiving a webhook event, the next step is fetching the corresponding **contact record** from Airtable.
   - Configuration:
     - **Base**: Contact List 1 (Scheduula).
     - **Table**: Chiropractors in UK (Google scraped @gmail.com).
     - The contact record is retrieved using the ID passed from the webhook.


3. **Email Routing (Router Module 3)**
   - The workflow branches out based on the **email schedule** for the contact.
   - This **router** checks which email (e.g., Email 1, Email 2) is due for the specific contact, based on the due date stored in Airtable.


4. **Send Email 1 (Module 2)**
   - If the **Email 1 schedule** matches the current date, an email is sent to the contact.
   - Configuration:
     - **To**: Contact's email.
     - **Subject**: Dynamic, includes the contact's name.
     - **HTML Content**: A predefined template with personalized content, including an unsubscribe link.


5. **Progress Update**
   - After sending the email, the **progress** field is updated to reflect the latest email sent (e.g., "Email 1 sent").
   - This ensures the system can pick up where it left off in future emails.


---


## Unsubscription Workflow


1. **Webhook Trigger (Unsubscribe)**
   - The unsubscription process is handled via a webhook that receives data when a contact clicks the unsubscribe link.
   - The webhook payload contains:
     - **Contact ID**
     - **Email address**
     - **Unsubscribe command**: `unsubscribe=yes`


2. **Update Airtable Record**
   - Once the webhook data is received, the system locates the contact in Airtable and updates the **Unsubscribed** field to "Yes."
   - Future emails will not be sent to unsubscribed contacts unless the status is manually reset.


---


## Cost Structure


- **Airtable**: $0 - $45/month
- **Make.com**: $0 - $182.16/month
- **Sendgrid**: $0 - $499/month


For up to **100k contacts** and **1M emails/month**, total costs are approximately **$726.16/month**, compared to MailChimp, HubSpot, ActiveCampaign, or Klaviyo, which charge significantly more for similar scale.


---


## Limitations


- **No Drag-and-Drop Email Editor**: Requires HTML/CSS knowledge for designing emails.
- **Manual Workflow Updates**: New email sequences must be added manually to Airtable.


---


## Conclusion


Scheduula offers a cost-effective and highly customizable email marketing solution that allows you to have complete control over your campaigns without restrictions. Though it requires a bit more setup than mainstream tools, the flexibility it provides makes it a valuable option for small businesses and developers alike.