# Redmine Check-in Automation

Redmine Check-in Automation is a DevOps workflow that automatically monitors Redmine for updated issues and sends notifications to a Mattermost team channel using n8n. The goal is to reduce manual checking of Redmine and give development teams faster visibility into important issue updates.

## Problem

Development teams often have to manually check Redmine to find out whether tasks or issues have changed. This can cause missed updates, delayed communication, unnecessary manual checking, and poor visibility across the team.

## Solution

This project uses n8n to automatically check Redmine for updated issues. When an issue contains a new update, the workflow retrieves the issue, checks its update history, formats the relevant information, and sends a notification to Mattermost.

## Architecture

```text
Redmine
   │
   │ REST API
   ▼
n8n
   │
   ├── Schedule Trigger
   │
   ├── Get Issues
   │
   ├── Split Issues
   │
   ├── Get Issue Details
   │
   ├── Check for Updates
   │
   ├── Format Message
   │
   └── HTTP Request
          │
          │ Webhook
          ▼
     Mattermost
          │
          ▼
   Team Notification
Technologies
Redmine
n8n
Mattermost
Docker
Docker Compose
MySQL
REST API
JavaScript
Webhooks
Workflow

The automation runs on a scheduled interval.

Schedule Trigger — n8n starts the workflow automatically.
Get Updated Issues — Retrieves issues from Redmine through the API.
Split Out — Processes each issue individually.
Get Issue Details — Retrieves complete issue information.
IF Node — Checks whether the issue contains update history.
Code in JavaScript — Formats the issue information into a readable notification.
HTTP Request — Sends the notification to Mattermost through a webhook.

The update condition is:

{{$json.issue.journals.length}} > 0
Example Notification
Redmine Issue Updated

Issue: Fix Kubernetes Deployment
Status: New
Priority: Normal
Assigned To: John Doe
Project: DevOps Team
Issue ID: 1
n8n Workflow Structure
Schedule Trigger
       ↓
Get Updated Issues
       ↓
Split Out
       ↓
Get Issue Details
       ↓
IF
       ↓
Code in JavaScript
       ↓
HTTP Request
       ↓
Mattermost
Docker Environment

The project uses Docker Compose to run the development environment.

Services:

Redmine
n8n
MySQL

Check the running containers:

docker compose ps

Start the services:

docker compose up -d

Stop the services:

docker compose down
Access

Redmine:

http://localhost:3000

n8n:

http://localhost:5678
Configuration

The project requires:

Redmine API access
Mattermost Incoming Webhook
n8n workflow configuration
Docker Desktop

Sensitive credentials should be stored securely.

Security

Never commit the following to GitHub:

API keys
Passwords
Mattermost webhook URLs
Database credentials
.env files
Other secrets

Use environment variables or n8n's credential management instead.

Testing

A Redmine issue can be updated, for example:

Fix Kubernetes Deployment

Add a note:

Monitoring progress updated.

Save the issue and execute the n8n workflow.

The workflow checks the issue journal, detects the update, formats the issue information, and sends it to Mattermost.

Expected Result

Mattermost receives:

Redmine Issue Updated

Issue: Fix Kubernetes Deployment
Status: New
Priority: Normal
Assigned To: John Doe
Project: DevOps Team
Issue ID: 1
Production Workflow

After successful testing, the Schedule Trigger can be configured to run every 5 minutes.

Redmine Issue Updated
        ↓
n8n detects update
        ↓
Issue information processed
        ↓
Mattermost notification
        ↓
Development team receives update

This allows the team to receive Redmine updates automatically without manually checking the project.

Screenshots

Project screenshots are available in the screenshots/ directory.

They demonstrate:

Redmine project and issues
n8n automation workflow
Redmine API data
Update detection
JavaScript message formatting
Mattermost notification
Successful workflow execution
Documentation

Additional project documentation is available in the docs/ directory:

architecture.md — System architecture and components
setup.md — Installation and setup instructions
workflow.md — Detailed n8n workflow
testing.md — Testing and validation procedures
DevOps Value

This project demonstrates practical DevOps automation by connecting issue management, REST APIs, workflow automation, scheduled execution, conditional processing, webhooks, team communication, and containerized infrastructure.

Instead of developers manually checking Redmine for changes, the automation continuously monitors the system and communicates relevant updates to the team.

Future Improvements
Automatic error notifications
Richer Mattermost notifications
Detection of newly created issues
Status-change notifications
Priority-based notifications
User-specific notifications
Workflow execution history
CI/CD integration
Automated reporting
More advanced Redmine event filtering
Project Structure
redmine-checkin-automation/
│
├── README.md
├── docker-compose.yml
│
├── docs/
│   ├── architecture.md
│   ├── setup.md
│   ├── workflow.md
│   └── testing.md
│
└── screenshots/
    ├── redmine-issues.png
    ├── n8n-workflow.png
    ├── issue-filter.png
    └── mattermost-notification.png
Author

Oluwaferanmi Dada

DevOps / Cloud Engineer

GitHub: feranzeey