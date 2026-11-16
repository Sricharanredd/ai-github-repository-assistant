
# AI GitHub Repository Assistant

An AI-powered GitHub issue management automation system that analyzes incoming issues, detects potential duplicates, and enables repository maintainers to review and approve or reject issues through Telegram.

## Overview

Managing GitHub issues manually can become time-consuming, especially when repositories receive repeated bug reports, feature requests, and questions.

The AI GitHub Repository Assistant automates the initial issue triage process using Google Gemini AI and Make.com. It analyzes newly created issues, checks for semantically similar existing issues, and sends the analysis to a Telegram bot for human review.

After the maintainer makes a decision, the system automatically updates the corresponding GitHub issue.

## Key Features

- **AI-powered issue classification:** Identifies issue type, priority, and severity.
- **Issue summarization:** Generates concise summaries and suggested responses.
- **Duplicate issue detection:** Compares incoming issues with existing open issues.
- **Telegram notifications:** Sends issue details and AI analysis to the repository maintainer.
- **Human-in-the-loop approval:** Allows maintainers to approve or reject issues through Telegram.
- **Automated GitHub updates:** Adds appropriate labels and comments based on the decision.
- **Decision tracking:** Uses Make Data Store to prevent repeated processing of the same issue.
- **Webhook-based automation:** Automatically processes newly opened GitHub issues.

## System Architecture

![System Architecture](docs/architecture/system-architecture.png)

## Technology Stack

| Technology | Purpose |
|---|---|
| GitHub | Issue management and repository webhooks |
| Make.com | Workflow automation and integration |
| Google Gemini AI | Issue classification and duplicate analysis |
| Telegram Bot API | Notifications and approval/rejection interface |
| Make Data Store | Decision tracking and duplicate-action prevention |

## Workflow

The project consists of two independent Make.com scenarios.

### Scenario 1: GitHub Issue Processor

Triggered whenever a new GitHub issue is opened.

1. GitHub sends an issue event through a webhook.
2. Make.com receives the issue information.
3. Gemini AI classifies the issue and generates a summary.
4. The system retrieves existing open GitHub issues.
5. An iterator and text aggregator prepare existing issues for comparison.
6. Gemini AI checks for semantically similar issues.
7. The result is parsed and routed as a duplicate or new issue.
8. Telegram sends the issue details and analysis to the maintainer.

### Scenario 2: Telegram Approval Handler

Triggered when the maintainer interacts with the Telegram inline buttons.

1. Telegram sends the callback event to Make.com.
2. The system checks whether the issue has already been processed.
3. Previously processed issues are blocked.
4. The approval or rejection decision is identified.
5. GitHub is updated according to the decision.
6. The corresponding comment and label are added where applicable.
7. The decision is saved in Make Data Store.

## Approval and Rejection Logic

| Decision | GitHub action |
|---|---|
| Approve new issue | Add `triaged` label and approval comment |
| Approve duplicate issue | Add `duplicate` label and approval comment |
| Reject issue | Add rejection comment |
| Repeat decision | Block further processing using Data Store |

The system follows a first-decision-wins approach: once an issue is processed, another decision through the same workflow is blocked.

## Repository Structure

```text
ai-github-repository-assistant/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── architecture/
│   │   └── system-architecture.png
│   ├── screenshots/
│   └── workflows/
│
├
│
└── examples/
    ├── sample-ai-analysis.json
    ├── sample-duplicate-issue.md
    └── sample-new-issue.md
```

## Setup and Configuration

### Prerequisites

- GitHub repository with permission to manage issues and webhooks.
- Make.com account.
- Google Gemini API access.
- Telegram bot created using BotFather.
- GitHub Personal Access Token with the required repository permissions.

### Configuration Steps

1. Clone or create a GitHub repository.
2. Create a custom webhook in Make.com.
3. Configure the GitHub repository webhook to send issue events.
4. Connect Google Gemini AI to Make.com.
5. Configure the GitHub API connection.
6. Create and connect a Telegram bot.
7. Configure both Make.com scenarios.
8. Activate both scenarios and test the complete workflow.

### Required Credentials

Configure credentials securely in Make.com connections. Never commit API keys, access tokens, bot tokens, or webhook secrets to GitHub.

## Example Inputs and Outputs

See the `examples/` directory for illustrative GitHub issue inputs and AI-generated analysis.

## Screenshots

Project workflow screenshots and execution results are available in:

- `docs/screenshots/`
- `docs/workflows/`

## Future Improvements

- More advanced semantic duplicate detection.
- Configurable issue classification rules.
- Support for additional GitHub events.
- Improved AI evaluation and confidence thresholds.
- Maintainer decision history and reporting.
- Support for multiple repositories.

## Project Status

Implemented and tested with live GitHub issue events, Gemini AI analysis, duplicate detection, Telegram notifications, and maintainer approval/rejection workflows.

---

**Built with GitHub, Make.com, Google Gemini AI, and Telegram.**
