# HubSpot Workflow Automation Guide

## Overview
This guide will help you create automated workflows triggered by your form submissions in HubSpot.

---

## Workflow 1: Contact Onboarding & Confirmation

### Purpose
Automatically send confirmation emails, notify team members, and initiate onboarding process.

### Step-by-Step Setup

#### 1. Create the Workflow

1. **Navigate to Workflows**
   - In HubSpot, go to `Automation` → `Workflows`
   - Click `Create workflow` (top right)
   - Select `Contact-based` workflow
   - Choose `Start from scratch`
   - Name it: "Form Submission - Contact Onboarding"

#### 2. Set the Enrollment Trigger

1. **Add Trigger**
   - Click `Set up triggers`
   - Select `Form submissions`
   - Choose your specific form name (e.g., "intake_form")
   - Set re-enrollment: `Off` (contacts enroll once)
   - Click `Apply filter`

2. **Additional Filters (Optional)**
   - Add filters to refine who enters the workflow
   - Example: `Contact owner is known` or `Lifecycle stage is any of Lead`

#### 3. Add Action 1: Send Confirmation Email

1. **Add Email Action**
   - Click the `+` button below the trigger
   - Select `Send email`
   
2. **Configure Email**
   - Click `Select or create an email`
   - Either:
     - **Create new email**: Click `Create new email`
       - Use template or design from scratch
       - Subject: "Thank you for your submission - We received your information"
       - Include personalization tokens: `{{contact.firstname}}`
       - Add relevant information about next steps
     - **Use existing email**: Select from your marketing emails
   
3. **Email Content Template**
   ```
   Subject: Thank you {{contact.firstname}} - We've received your submission
   
   Hi {{contact.firstname}},
   
   Thank you for submitting your information through our form. We've successfully 
   received your details and our team is reviewing them.
   
   What happens next?
   - Our team will review your submission within 24 hours
   - You'll receive a follow-up email with next steps
   - A dedicated account manager will be assigned to you
   
   If you have any questions, feel free to reply to this email.
   
   Best regards,
   [Your Company Name]
   ```

#### 4. Add Action 2: Notify Internal Team

1. **Add Internal Notification**
   - Click the `+` button
   - Select `Send internal email notification`
   
2. **Configure Notification**
   - To: Add team member emails (sales team, account managers)
   - Subject: "New Form Submission - {{contact.firstname}} {{contact.lastname}}"
   - Body:
     ```
     New contact submitted the form!
     
     Name: {{contact.firstname}} {{contact.lastname}}
     Email: {{contact.email}}
     Phone: {{contact.phone}}
     Company: {{contact.company}}
     
     Form submitted: [Form Name]
     Submission date: {{contact.createdate}}
     
     View contact: [Contact Record Link]
     ```

#### 5. Add Action 3: Assign Contact Owner

1. **Add Owner Assignment**
   - Click the `+` button
   - Select `Rotate record to owner`
   
2. **Configure Assignment**
   - Property: `Contact owner`
   - Choose assignment method:
     - **Rotate evenly**: Distributes contacts equally among team
     - **Assign to specific user**: Choose one team member
     - **Least recently assigned**: Balances workload
   - Add team members to rotation list

#### 6. Add Action 4: Add to List/Segment

1. **Add List Enrollment**
   - Click the `+` button
   - Select `Enroll in another workflow` OR `Add to list`
   
2. **Create/Select List**
   - If creating new list:
     - Go to `Contacts` → `Lists`
     - Click `Create list`
     - Name: "Form Submissions - Intake Form"
     - Type: `Active list`
     - Filter: `Form submissions` → Your form name
   
   - Back in workflow, select your list
   - This segments contacts for targeted campaigns

#### 7. Add Action 5: Update Lifecycle Stage

1. **Set Property Value**
   - Click the `+` button
   - Select `Set property value`
   
2. **Configure**
   - Property: `Lifecycle stage`
   - Value: `Lead` (or `Marketing Qualified Lead`)
   - This tracks contact progression

#### 8. Add Action 6: Add to Pipeline (If using Deals)

1. **Create Deal**
   - Click the `+` button
   - Select `Create record`
   - Record type: `Deal`
   
2. **Configure Deal**
   - Deal name: `{{contact.firstname}} {{contact.lastname}} - Onboarding`
   - Pipeline: Select your custom pipeline (e.g., "Onboarding Pipeline")
   - Deal stage: "New Submission" or "Document Collection"
   - Amount: (if applicable)
   - Associated contact: Current contact

---

## Workflow 2: Follow-up Tasks & Document Collection

### Purpose
Automate task creation for document collection, scheduling, and follow-ups.

### Step-by-Step Setup

#### 1. Create Second Workflow

1. **Create Workflow**
   - Go to `Automation` → `Workflows`
   - Click `Create workflow`
   - Select `Contact-based`
   - Name: "Form Submission - Follow-up Tasks"

#### 2. Set Enrollment Trigger

1. **Same as Workflow 1**
   - Trigger: `Form submissions` → Your form
   - Or trigger: `Contact enters list` → Select your form submissions list

#### 3. Add Delay (Optional)

1. **Add Delay**
   - Click the `+` button
   - Select `Delay`
   - Set delay: `1 hour` (gives time for initial email to be sent)

#### 4. Action 1: Create Task - Document Collection

1. **Add Task**
   - Click the `+` button
   - Select `Create task`
   
2. **Configure Task**
   - Task type: `To-do`
   - Title: "Collect required documents from {{contact.firstname}} {{contact.lastname}}"
   - Description:
     ```
     Required documents to collect:
     - [ ] ID verification
     - [ ] Proof of address
     - [ ] Additional form information
     
     Contact: {{contact.email}} | {{contact.phone}}
     ```
   - Assigned to: `Contact owner` or specific team member
   - Priority: `High`
   - Due date: `2 days from now`
   - Queue: Select appropriate queue

#### 5. Add Delay Before Next Task

1. **Add Delay**
   - Click the `+` button
   - Select `Delay`
   - Set: `3 days`

#### 6. Action 2: Create Task - Onboarding Scheduling

1. **Add Task**
   - Click the `+` button
   - Select `Create task`
   
2. **Configure**
   - Title: "Schedule onboarding call with {{contact.firstname}} {{contact.lastname}}"
   - Description:
     ```
     Schedule initial onboarding session:
     - Review collected documents
     - Explain next steps
     - Answer questions
     - Set up account details
     
     Contact: {{contact.email}} | {{contact.phone}}
     ```
   - Assigned to: `Contact owner`
   - Due date: `5 days from now`

#### 7. Add Branch for Conditional Follow-up

1. **Add If/Then Branch**
   - Click the `+` button
   - Select `If/then branch`
   
2. **Configure Branch**
   - Condition: `Onboarding call scheduled is not known`
   - This checks if task was completed

3. **Add Reminder Email (If not scheduled)**
   - In the "Yes" branch, add:
   - Action: `Send email`
   - Email: "Reminder - Schedule your onboarding call"
   - Content: Gentle reminder to book appointment

---

## Workflow 3: Monthly Fee Reminder (Recurring)

### Purpose
Automate monthly fee payment reminders.

### Step-by-Step Setup

#### 1. Create Workflow

1. **Create Workflow**
   - Go to `Automation` → `Workflows`
   - Name: "Monthly Fee Reminder - Recurring"
   - Type: `Contact-based`

#### 2. Set Enrollment Trigger

1. **Trigger Options**
   - Option A: `List membership` → Contacts who are active clients
   - Option B: `Property value` → Lifecycle stage = Customer
   - **Set re-enrollment**: `Yes` - This allows recurring notifications
   - Re-enrollment criteria: `Every month on day 25` (or your billing date)

#### 3. Add Action: Send Fee Reminder Email

1. **Send Email**
   - Click `+` → `Send email`
   - Create email with subject: "Payment Reminder - Monthly Fee Due"
   - Content:
     ```
     Hi {{contact.firstname}},
     
     This is a friendly reminder that your monthly fee of $[AMOUNT] is due on [DATE].
     
     Payment methods:
     - Online portal: [LINK]
     - Bank transfer: [DETAILS]
     - Direct debit: Automatic on file
     
     If you've already paid, please disregard this message.
     
     Questions? Contact us at [EMAIL/PHONE]
     
     Thank you!
     ```

#### 4. Add Task for Team Follow-up

1. **Create Task**
   - Click `+` → `Create task`
   - Title: "Verify payment received - {{contact.firstname}} {{contact.lastname}}"
   - Assigned to: Billing team
   - Due date: `3 days from now`

#### 5. Add Branch for Non-Payment

1. **Add Delay**
   - Click `+` → `Delay`
   - Set: `7 days`

2. **Add If/Then Branch**
   - Condition: `Last payment date is more than 30 days ago`
   - Or custom property: `Payment status is not Paid`

3. **Add Follow-up Action**
   - In "Yes" branch: Send escalation email or create high-priority task

---

## Additional Workflow Actions You Can Add

### 1. **SMS Notifications** (If HubSpot SMS enabled)
- Add SMS action for urgent notifications
- Example: "Thank you for submitting! Check your email for next steps."

### 2. **Update Custom Properties**
- Set property: `Onboarding status` = "In Progress"
- Set property: `Source` = "Website Form"
- Set property: `Last interaction date` = Current date

### 3. **Create Meeting Link**
- Add action: `Book meeting`
- Automatically send scheduling link
- Sync with calendar

### 4. **Webhook to External System** (Advanced)
- Add action: `Trigger webhook`
- Send data to external CRM, accounting software, or custom database

### 5. **Lead Scoring**
- Add action: `Increase contact score`
- Award points for form completion
- Helps prioritize hot leads

---

## Testing Your Workflows

### Before Activating

1. **Review All Actions**
   - Check each step is configured correctly
   - Verify email templates are finalized
   - Confirm team member assignments

2. **Test with Sample Contact**
   - Click `Test` button (top right in workflow editor)
   - Enter a test contact email (your own)
   - Walk through each action to verify

3. **Check Enrollment Criteria**
   - Review trigger conditions
   - Verify re-enrollment settings
   - Confirm timing of delays

### Activate Workflow

1. Click `Review and publish` (top right)
2. Review summary of actions
3. Click `Turn on` to activate

---

## Monitoring & Optimization

### Check Workflow Performance

1. **View Workflow Analytics**
   - Go to your workflow
   - Click `Performance` tab
   - Monitor:
     - Enrollment count
     - Completion rate
     - Email open/click rates
     - Task completion rates

2. **Set Up Notifications**
   - Get alerts for workflow errors
   - Monitor enrollments
   - Track goal completions

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Contacts not enrolling | Check trigger filters, verify form is correct |
| Emails not sending | Verify email is published, check contact has valid email |
| Tasks not assigned | Ensure contact owner is set, check team member permissions |
| Duplicate enrollments | Turn off re-enrollment if not needed |

---

## Best Practices

### ✅ Do's

- **Test thoroughly** before activating
- **Use clear naming** conventions for workflows
- **Set appropriate delays** between actions (don't overwhelm contacts)
- **Monitor regularly** and adjust based on performance
- **Document your workflows** (what they do, why they exist)
- **Use personalization tokens** in emails for better engagement
- **Set goals** to measure workflow success

### ❌ Don'ts

- Don't create too many parallel workflows (can confuse contacts)
- Don't send emails too frequently (risk unsubscribes)
- Don't forget to update workflows when processes change
- Don't skip testing phase
- Don't use workflows for one-time bulk operations (use lists instead)

---

## Workflow Templates Summary

Here's what we've created:

### Workflow 1: Contact Onboarding
- **Trigger**: Form submission (intake_form)
- **Actions**:
  1. Send confirmation email to contact
  2. Send internal notification to team
  3. Assign contact owner
  4. Add to segmented list
  5. Update lifecycle stage
  6. Create deal in pipeline

### Workflow 2: Follow-up Tasks
- **Trigger**: Form submission
- **Actions**:
  1. Delay 1 hour
  2. Create document collection task
  3. Delay 3 days
  4. Create onboarding scheduling task
  5. Branch: If not scheduled → Send reminder

### Workflow 3: Monthly Fee Reminder
- **Trigger**: Active customers (recurring monthly)
- **Actions**:
  1. Send fee reminder email
  2. Create payment verification task
  3. Delay 7 days
  4. Branch: If unpaid → Escalate

---

## Next Steps

1. ✅ Set up Workflow 1 (Contact Onboarding)
2. ✅ Create email templates for confirmations
3. ✅ Configure team member assignments
4. ✅ Set up Workflow 2 (Follow-up Tasks)
5. ✅ Test both workflows with sample data
6. ✅ Activate workflows
7. ✅ Set up Workflow 3 (Monthly Fee Reminder) if needed
8. ✅ Monitor performance after 1 week
9. ✅ Optimize based on results

---

## Support Resources

- **HubSpot Academy**: Free workflow training courses
- **HubSpot Community**: Ask questions and get help
- **HubSpot Support**: Contact for technical issues
- **Documentation**: https://knowledge.hubspot.com/workflows

---

## Questions to Consider

Before finalizing your workflows, answer these:

1. **What is your typical response time to form submissions?**
   - Adjust delays accordingly

2. **Who should own new contacts?**
   - Set up rotation or specific assignment

3. **What documents do you need to collect?**
   - Customize task descriptions

4. **When are fees due each month?**
   - Set re-enrollment date for payment reminders

5. **What defines a successful onboarding?**
   - Set workflow goals to track this

---

**Need help implementing these workflows? Let me know which specific workflow you'd like to set up first, and I can provide more detailed guidance!**
