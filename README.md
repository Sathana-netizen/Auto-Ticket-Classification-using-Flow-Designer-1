# Auto Ticket Classification Using Flow Designer — Complete Project Overview

## 1. Project introduction

Auto Ticket Classification using Flow Designer is an IT Service Management (ITSM) automation project that automatically categorizes incoming service tickets, assigns them to the appropriate support team, and helps reduce manual work in ticket management.

This project is commonly implemented using ServiceNow Flow Designer, a low-code automation tool used to create workflows and automate business processes.

In a typical IT support environment, employees raise tickets for issues such as:

- Laptop or desktop problems
- Network connectivity issues
- Software installation requests
- Email and login problems
- Printer failures
- Access and permission requests

Normally, a support agent reads each ticket, identifies its category, sets its priority, and assigns it to the correct team. This process can be slow and may result in incorrect assignments.

The proposed project automates these steps using predefined conditions and workflow logic.

Flow Designer – No-Code Workflows - ServiceNow

Effective Help Desk Ticket Workflow: Your Setup Guide - Screendesk Blog

Project goal: Automatically classify service tickets based on their details and route them to the correct support group with minimal manual intervention.

## 2. Problem statement

In a traditional ticket management system, incoming tickets are often classified manually by the service desk team.

This creates several problems:

- Time consumption: Agents must read and categorize every ticket.
- Incorrect classification: A ticket may be assigned to the wrong category or support group.
- Delayed resolution: Incorrect assignments cause unnecessary transfers between teams.
- Inconsistent prioritization: Similar incidents may receive different priorities.
- Increased workload: Support teams spend time on repetitive administrative tasks.

### Proposed solution

Develop a Flow Designer workflow that triggers when a new ticket is created or when relevant ticket details change. The workflow evaluates ticket information, identifies the appropriate category and assignment group, and updates the ticket automatically.

Depending on the implementation, the system can also determine priority, send notifications, and route uncertain cases to a human agent.

## 3. Objectives of the project

The main objectives are:

1. Automatically classify incoming tickets based on their short description and other available fields.
2. Assign tickets to the correct support group.
3. Reduce manual intervention in ticket processing.
4. Apply consistent classification and assignment rules.
5. Improve ticket routing speed and reduce avoidable reassignment.
6. Maintain an audit trail of automated decisions.
7. Provide a workflow that can be expanded with AI-based classification in the future.

## 4. Technologies used

Free High-Quality Servicenow Logo for Creative Design

ServiceNow platform

The main platform for creating, storing, managing, and tracking incident tickets.

ServiceNow Developers

Flow Designer / Workflow Studio

Creates the automated workflow using triggers, conditions, actions, and flow logic. The exact interface name depends on your ServiceNow release. ServiceNow

+1

Field decorator is not showing in created incident... - ServiceNow Community

Incident Management

Stores ticket information, including short description, category, subcategory, impact, urgency, priority, and assignment group. ServiceNow

+1

Decision Table Builder - ServiceNow Store

Conditions and decision logic

Maps ticket details to categories and support groups. You can implement this with flow branches or a decision table, depending on the project design.

Optional technologies include JavaScript for advanced logic, email notifications, IntegrationHub for external integrations, and AI-based classification for interpreting free-text descriptions.

Important distinction: Flow Designer does not automatically understand the meaning of every sentence by itself. A basic project uses rules that you configure. Genuine AI-based classification requires an additional AI capability, model, or integration.

## 5. How the project works

The project follows a simple sequence from ticket creation to automated assignment.

1. User creates a ticket

Enters a short description and issue details

2. Flow is triggered

A new incident record satisfies the trigger conditions

3. Analyze ticket details

Evaluate the description, category, keywords, or other fields

4. Apply classification rules

Choose the category, subcategory, and destination group

5. Update the incident

Write the classification and assignment values to the record

6. Notify and track

Optionally notify the team and verify the result

ServiceNow supports record-based triggers and reusable workflow actions for this type of automation. ServiceNow

+1

### Example of automated classification

Suppose an employee submits this ticket:

Sample incident

INCIDENT

Short description: "Unable to connect to office Wi-Fi"

Description: "My laptop cannot connect to the company wireless network."

Expected output after the flow runs

Category: Network

Subcategory: Wireless

Assignment group: Network Support

The flow identifies the matching rule and updates the incident. The category and subcategory are consistent with ServiceNow's documented incident classification options; `Network Support` is an illustrative group name that you would configure in your instance. ServiceNow

+1

## 6. Classification rules and examples

For a beginner-friendly project, you can build a rule-based classification system.

| Ticket description or keyword | Category | Subcategory | Assignment group |
| --- | --- | --- | --- |
| Wi-Fi not working | Network | Wireless | Network Support |
| VPN connection failed | Network | VPN | Network Support |
| Laptop screen not working | Hardware | Monitor | Hardware Support |
| Keyboard is damaged | Hardware | Keyboard | Hardware Support |
| Cannot open an application | Software | Internal Application | Application Support |
| Email is not working | Inquiry / Help | Email | Email Support |
| Cannot log in | Inquiry / Help | Login or Access\* | Service Desk |

\*Example subcategories and group names may need to be created or adapted to match the categories, subcategories, and assignment groups configured in your ServiceNow instance.

### How the rules work

For example, a flow might evaluate these conditions:

- If the short description contains `Wi-Fi`, `wireless`, or `network connection`, classify it as Network.
- If it contains `monitor`, `keyboard`, or `screen`, classify it as Hardware.
- If it contains `application error` or `software crash`, classify it as Software.
- If it contains `email`, route it to the appropriate email-support team.
- If no rule matches, leave the ticket for manual classification by the service desk.

These are illustrative rules. In a production system, you should also consider ambiguous words, multiple matching keywords, spelling mistakes, and conflicting conditions.

## 7. How to implement the project in ServiceNow

Here is a practical implementation plan you can follow in a ServiceNow developer instance.

### Step 1: Prepare the environment

- Log in to your ServiceNow instance.
- Open the application navigator.
- Navigate to Incident \> All or the equivalent Incident Management module.
- Confirm that you can create and update incident records.
- Check that the required categories, subcategories, and assignment groups exist.

For this project, you will generally work with the Incident table, commonly named `incident`, which extends the Task table.

### Step 2: Create sample tickets

Create test incidents with different descriptions, such as:

- "Wi-Fi connection is not working."
- "My laptop keyboard is damaged."
- "Email application keeps crashing."
- "VPN connection fails from home."
- "Unable to access the internal portal."

Initially, you can leave the category and assignment group unpopulated so that you can observe whether the flow classifies them correctly.

### Step 3: Create a new flow

Navigate to Process Automation \> Workflow Studio or the Flow Designer entry point available in your instance.

1. Select New \> Flow.
2. Enter a name such as `Auto Ticket Classification`.
3. Add a description explaining the workflow.
4. Select the appropriate application scope.
5. Configure the trigger.

The exact navigation can vary by ServiceNow release. The official documentation explains the flow creation process. ServiceNow

+1

### Step 4: Configure the trigger

For the basic version, configure a record trigger with settings similar to these:

| Setting | Example configuration |
| --- | --- |
| Table | Incident `[incident]` |
| Trigger | Created |
| Conditions | Short description is not empty |
| Purpose | Process each new ticket with a description |

A creation trigger is a good starting point because it prevents unnecessary reclassification every time an agent updates an existing ticket.

If your use case requires reclassification after the description changes, add a carefully controlled update trigger instead.

### Step 5: Add classification conditions

Inside the flow, use flow logic such as If, Else If, and Else to evaluate ticket details.

Example:

text
```

IF Short Description contains "Wi-Fi"
    Category = Network
    Subcategory = Wireless
    Assignment Group = Network Support

ELSE IF Short Description contains "VPN"
    Category = Network
    Subcategory = VPN
    Assignment Group = Network Support

ELSE IF Short Description contains "keyboard"
    Category = Hardware
    Subcategory = Keyboard
    Assignment Group = Hardware Support

ELSE IF Short Description contains "email"
    Category = Inquiry / Help
    Subcategory = Email
    Assignment Group = Email Support

ELSE
    Mark for manual classification
```

This is illustrative pseudocode, not a ready-to-run ServiceNow script. Configure the conditions using the available flow logic and the actual field choices in your instance.

### Step 6: Update the incident record

After the appropriate branch matches, add an Update Record action.

- Select the triggering Incident record.
- Set the Category field.
- Set the Subcategory field.
- Set the Assignment group field.
- Optionally add a work note explaining that the incident was automatically classified.

Use the triggering record's data pills to populate the action inputs rather than creating an unnecessary new incident.

Important: Select the assignment group using the actual group reference from your instance. A text value such as `Network Support` is not necessarily sufficient to populate a reference field.

### Step 7: Activate the flow

- Save the flow.
- Validate the trigger and action configuration.
- Activate the flow.
- Create a new test incident.
- Wait for the flow execution to finish.
- Reopen the incident and verify the field values.

Use the flow execution details to inspect failures and identify actions that did not run as expected. ServiceNow

+1

## 8. Flow Designer components used in the project

| Component | Purpose |
| --- | --- |
| Trigger | Starts the workflow when a qualifying incident is created or updated. |
| Flow logic | Selects the correct classification branch. |
| Conditions | Checks ticket fields and matching rules. |
| Data pills | Passes incident values between actions. |
| Update Record | Saves the category and assignment information. |
| Send Notification (optional) | Notifies a support group or ticket requester. |
| Error handling | Helps manage failed or incomplete workflow actions. |

The trigger, flow logic, actions, and data pills are the core building blocks of this design. ServiceNow

+1

## 9. Optional enhancement: Automatic priority assignment

You can extend the project to recommend or assign priority based on the ticket's impact and urgency.

For example:

- A widespread network outage affecting many employees may require high priority.
- A single employee's minor keyboard issue may receive a lower priority.
- A complete business-critical service outage may need immediate attention.

In ServiceNow, priority is based on impact and urgency, so the project should follow the priority configuration in your instance rather than assigning arbitrary priority values. ServiceNow

A sensible sequence is:

1. Classify the ticket.
2. Determine the assignment group.
3. Check impact and urgency.
4. Apply the configured priority logic.
5. Notify the appropriate team if needed.

Keep this enhancement separate from basic classification until the first workflow works reliably.

## 10. Optional enhancement: AI-based ticket classification

A rule-based system is sufficient for a basic academic project. However, a more advanced version can use AI to understand ticket descriptions.

Consider these descriptions:

- "The office wireless keeps dropping."
- "I lose connectivity whenever I enter the meeting room."
- "My device can see the Wi-Fi but cannot connect."

All three may refer to a wireless-network issue, even though they use different words.

A keyword-based workflow may miss some of these cases. An AI classification component can potentially interpret the meaning and suggest a category, subcategory, or assignment group.

| Rule-based classification | AI-based classification |
| --- | --- |
| Uses predefined conditions | Uses a trained or configured model |
| Easier to implement | Requires additional setup |
| Predictable for matching rules | Can handle varied wording more effectively |
| May miss unexpected descriptions | May still misclassify ambiguous cases |
| Ideal for a beginner project | Useful as an advanced extension |

AI capabilities and licensing depend on the ServiceNow products available to your organization. Do not describe a keyword-based flow as machine learning unless it actually uses a model.

## 11. Testing and validation

Testing is essential because a flow can execute successfully while assigning an incorrect category or support group.

Create a test plan covering normal, ambiguous, and unmatched tickets.

| Test case | Input | Expected result |
| --- | --- | --- |
| TC01 | Wi-Fi not connecting | Network → Wireless |
| TC02 | VPN connection failed | Network → VPN |
| TC03 | Laptop keyboard damaged | Hardware → Keyboard |
| TC04 | Email application error | Inquiry / Help → Email |
| TC05 | Unknown issue description | Manual classification |
| TC06 | Empty short description | Flow should not classify the ticket |
| TC07 | Ticket already manually classified | Follow the configured overwrite policy |
| TC08 | Assignment group is unavailable | Handle safely; do not silently claim successful assignment |

For each test, verify:

- Whether the trigger ran.
- Whether the correct branch was selected.
- Whether the category and subcategory were updated.
- Whether the assignment group is correct.
- Whether the ticket was updated only as intended.
- Whether errors were recorded and could be investigated.

### Measuring project performance

You can evaluate the project using the following metrics:

- Classification accuracy: Percentage of tickets assigned the correct category.
- Assignment accuracy: Percentage of tickets routed to the correct group.
- Automation rate: Percentage of eligible tickets processed without manual classification.
- Reassignment rate: Percentage of tickets transferred to another group because the original assignment was incorrect.
- Processing time: Time taken to classify and route a ticket.

For example, if 90 out of 100 test tickets are classified correctly, the classification accuracy is 90%. This is an illustrative calculation, not a measured result for your project.

## 12. Advantages of the project

Time saving

Eliminates repetitive manual classification for tickets that match the configured rules.

Faster routing

Sends incidents to the appropriate support group without requiring an agent to select the group manually.

Consistent classification

Applies the same rules to similar tickets, improving reporting and workflow consistency.

Low-code implementation

Uses a visual workflow builder instead of requiring a large amount of custom code.

Better operational visibility

Consistent categories help teams analyze incident trends and identify recurring problems.

## 13. Limitations and challenges

A good project explanation should also acknowledge the limitations.

- Keyword ambiguity: The word "network" may occur in a ticket that is not actually a network problem.
- Multiple matching rules: A description may mention hardware, software, and connectivity issues together.
- Incorrect reference values: An invalid or unavailable assignment group can prevent the intended assignment.
- Existing automation: Business rules, assignment rules, or other flows may also update the same fields.
- Unmatched tickets: New or unusual problems may not match any configured condition.
- Maintenance: Classification rules need updating as categories, support teams, and services change.

To address these issues, define a rule precedence, provide a manual-review fallback, test interactions with existing automation, and maintain an audit trail. ServiceNow's incident management documentation also emphasizes the role of categorization in routing and reporting. ServiceNow

+2

## 14. Future enhancements

Once the basic version works, you can expand it with the following features:

- AI-based classification of natural-language descriptions.
- Automatic priority recommendations based on impact and urgency.
- Duplicate incident detection.
- Automatic notifications to the assigned support group.
- Automatic detection of recurring incidents.
- A dashboard showing classification accuracy and ticket volumes.
- Support for additional ticket types and service request workflows.
- Human approval or review for uncertain classifications.

A particularly useful enhancement is a confidence-based fallback: tickets that the classifier cannot confidently categorize are sent to a human agent rather than being assigned automatically.

## 15. Expected project output

After successful implementation, the expected result is a ServiceNow instance in which:

- A user creates an incident.
- The flow starts automatically.
- The incident description is evaluated against configured classification rules.
- The category and subcategory are populated.
- The incident is assigned to the intended support group.
- Unmatched tickets remain available for manual review.
- The workflow execution can be inspected for troubleshooting.

The actual output should be demonstrated through test incidents and execution records rather than assumed before testing.
## 16. Final project summary

| Item | Description |
| --- | --- |
| Project title | Auto Ticket Classification Using Flow Designer |
| Platform | ServiceNow |
| Main tool | Flow Designer / Workflow Studio |
| Main table | Incident (`incident`) |
| Input | Ticket short description and other incident details |
| Processing | Predefined conditions and classification rules |
| Output | Category, subcategory, and assignment group |
| Optional features | Priority assignment, notifications, AI classification |
| Main benefit | Reduced manual classification and more consistent ticket routing |

Recommended approach: If this is your college mini-project or final-year project, first build the rule-based version using Flow Designer, demonstrate at least five successful test cases, and then present AI-based classification as an enhancement unless you actually implement it.

For implementation details, refer to the official ServiceNow documentation for [Flow Designer and Workflow Studio](<https://www.servicenow.com/docs/r/build-workflows/workflow-studio/exploring-flows.html>) and [Incident Categories and Subcategories](<https://www.servicenow.com/docs/r/it-service-management/incident-management/r_CategorizingIncidents.html>).
