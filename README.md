# Redmine Check-in Automation

An automated DevOps workflow that monitors Redmine issues and sends scheduled notifications to a Mattermost team channel using n8n.

The project connects Redmine, n8n, and Mattermost to reduce manual issue checking and give development teams faster visibility into task activity and updates.

---

## Project Overview

Development teams often use Redmine for issue and project management while communicating through separate collaboration platforms such as Mattermost.

Without automation, team members may need to repeatedly check Redmine to discover:

- Issue updates
- Status changes
- Assignment changes
- Priority changes
- Task activity
- New issue activity

This project automates that communication flow.

The n8n workflow periodically retrieves Redmine issues through the REST API, processes the issue information, checks for update activity, formats the relevant details using JavaScript, and sends a notification to Mattermost.

---

## Architecture

![Redmine Check-in Automation Architecture](screenshots/redmine-checkin-architecture.png)

## How It Works

The workflow follows this process:

1. The Schedule Trigger starts the workflow.
2. n8n retrieves Redmine issues through the REST API.
3. Each issue is processed individually.
4. n8n retrieves detailed issue information.
5. The workflow checks the issue journal for update activity.
6. An IF condition determines whether the issue should continue.
7.JavaScript formats the issue information into a readable notification.
8. An HTTP Request sends the notification to the configured Mattermost Incoming Webhook.
9. The development team receives the notification in Mattermost.

---

## Workflow

```text
Schedule Trigger
       │
       ▼
Get Redmine Issues
       │
       ▼
Split Issues
       │
       ▼
Get Issue Details
       │
       ▼
Check Issue Updates
       │
       ▼
      IF
       │
       │ True
       ▼
Code in JavaScript
       │
       ▼
HTTP Request
       │
       ▼
Mattermost
       │
       ▼
Team Notification
```

---

## Update Detection

The current workflow checks the Redmine issue journal for update activity.

{{$json.issue.journals.length}}

The condition used by the workflow is:

journals.length > 0

This allows issues containing journal activity to continue through the notification workflow.
```

Note: This implementation checks whether an issue has journal activity. A future improvement is to track the last processed updated_on timestamp so that the workflow only notifies the team about changes that occurred since the previous execution.

---

## Example Notification

A Mattermost notification can contain information such as:

Redmine Issue Updated

Issue: Fix Kubernetes Deployment
Status: New
Priority: Normal
Assigned To: John Doe
Project: DevOps Team
Issue ID: 1

This provides the team with important issue information without requiring them to manually open Redmine.

---

## Technologies

| Technology | Purpose |
|---|---|
| Redmine | Issue and project management |
| n8n | Workflow automation |
| Mattermost | Team notifications |
| Docker | Containerization |
| Docker Compose | Multi-container environment |
| MySQL | Redmine database |
| JavaScript | Message processing |
| REST API | Redmine integration |
| Webhooks | Mattermost notification delivery |

---

## Docker Environment

The project uses Docker Compose to run the local environment.

### Services

```text
Redmine
n8n
MySQL
```

Check running containers:

```bash
docker compose ps
```

Start the environment:

```bash
docker compose up -d
```

Stop the environment:

```bash
docker compose down
```

View container logs:

```bash
docker compose logs
```

View logs for a specific service:

```bash
docker compose logs redmine
```

```bash
docker compose logs n8n
```

---

## Application Access

### Redmine

```text
http://localhost:3000
```

### n8n

```text
http://localhost:5678
```

---

## Project Structure

```text
redmine-checkin-automation/
│
├── README.md
├── docker-compose.yml
│
├── Workflows/
│   ├── Automated Redmine check-in-automation
│
│
└── screenshots/
    ├── n8n-with-issues-tracking.png
    ├── n8n-docker-installation-success.png
    ├── n8n-redmine-api-configuration.png
    ├── n8n-redmine-api-request.png
    ├── n8n-redmine-mattermost-workflow.png
    ├── n8n-scheduled-workflow-publish.png
    ├── redmine-issue-categories.png
    ├── redmine-project-settings.png
    └── redmine-user-activity.png
```

---

## Configuration

The local environment requires:

- Docker Desktop
- Redmine
- n8n
- MySQL
- Redmine REST API access
- Mattermost Incoming Webhook

Credentials should be stored securely using:

- n8n credentials
- Environment variables
- Docker secrets
- Secure deployment configuration

Never hard-code credentials directly inside workflow nodes or source files.

---

## Security

Never commit sensitive credentials to GitHub.

Sensitive information includes:

API keys
Passwords
Mattermost webhook URLs
Database passwords
Access tokens
Private keys
.env files
Credential files

If a secret is accidentally committed or exposed, revoke or regenerate it immediately.
```

Use `.gitignore` to prevent accidental commits of sensitive files.

Example:

The repository should include a .gitignore similar to:

# Environment variables
.env
.env.*
!.env.example

# Credentials and secrets
credentials.json
secrets/
*.key
*.pem
*.crt

# n8n local data
.n8n/

# Logs
*.log
logs/

# Operating system files
.DS_Store
Thumbs.db

# IDE files
.vscode/
.idea/
```

---

## Testing

To test the automation:

### Step 1 — Open Redmine

```text
http://localhost:3000
```

### Step 2 — Open an existing issue

For example:

```text
Fix Kubernetes Deployment
```

### Step 3 — Update the Issue

Example:

```text
Monitoring progress updated.
```

Save the issue.

### Step 4 — Run the n8n workflow

Execute the workflow manually during testing.

### Step 5 — Verify Mattermost

The updated issue should appear automatically in the configured Mattermost channel.

---

## Scheduled Automation

After successful testing, the n8n Schedule Trigger can be configured to run every 5 minutes.

The scheduled workflow becomes:

```text
Redmine
   │
   ▼
Scheduled n8n Workflow
   │
   ▼
Detect Issue Updates
   │
   ▼
Process Issue
   │
   ▼
Send Mattermost Notification
   │
   ▼
Development Team
```

This allows the team to receive automated updates without repeatedly checking Redmine manually.

---

## Screenshots

The `screenshots/` directory contains visual documentation of the implementation, including:

- Redmine issue management
- Redmine project configuration
- Redmine user activity
- n8n Docker deployment
- Redmine API configuration
- Redmine API requests
- n8n workflow automation
- Scheduled workflow publishing
- Issue tracking automation

---

## Key DevOps Concepts Demonstrated

This project demonstrates practical experience with:

- Workflow automation
- REST API integration
- Docker containerization
- Docker Compose
- Scheduled automation
- Conditional workflow logic
- JavaScript automation
- HTTP APIs
- Webhooks
- Issue tracking
- Team notifications
- Infrastructure automation
- Secure credential management

---

## Business Value

The automation reduces repetitive manual work by automatically communicating Redmine changes to the development team.

Instead of:

```text
Engineer
   ↓
Open Redmine
   ↓
Check Issues
   ↓
Find Changes
   ↓
Notify Team
```

The automated process is:

```text
Redmine
   ↓
n8n detects update
   ↓
Issue processed
   ↓
Mattermost notification
   ↓
Team informed
```

This improves visibility, reduces manual checking, and helps teams respond to changes faster.

---

## mplemented Features

Redmine REST API integration 
✓ n8n workflow automation 
✓ Scheduled issue retrieval 
✓ Issue-by-issue processing 
✓ Issue journal/activity detection
✓ Conditional workflow logic
✓ JavaScript message formatting
✓ Mattermost notifications
✓ Docker-based local environment
✓ Docker Compose
✓ Secure credential configuration


## Future Improvements

Potential improvements include:

- Track updated_on timestamps
- Prevent duplicate notifications
- Detect specific status changes
- Detect priority changes
- Detect assignment changes
- Detect newly created issues
- Rich Mattermost messages
- User-specific notifications
- Redmine webhook triggers
- Automated daily reports
- Workflow monitoring
- Error recovery
- CI/CD integration
- Automated testing

---

## Skills Demonstrated

```text
Docker
Docker Compose
n8n
Redmine
Mattermost
REST APIs
JavaScript
Webhooks
Workflow Automation
DevOps
Infrastructure Automation
```

---

## Author

**Oluwaferanmi Dada**

DevOps / Cloud Engineer

GitHub: **feranzeey**

---

## Project Goal

The goal of this project is simple:

> Automatically detect Redmine issue updates and make sure the right team members know about them without manually checking Redmine.

This project demonstrates how DevOps automation can connect development tools together to create a faster and more reliable engineering workflow.