# Automation & QA Assessment

**Candidate:** Rohan Salunkhe  
**Project:** Website QA, API Automation, and Uptime Monitoring  
**Platform:** n8n

## Project Overview

This submission contains three parts:

1. **Task 1 – Website QA:** A QA report documenting website testing findings and root-cause analysis.
2. **Task 2 – Daily Morning Brief workflow:** An n8n workflow that retrieves posts and author information from JSONPlaceholder, selects five posts, separates them by title length, and emails two digests. It also includes an API-request failure notification path.
3. **Bonus – Uptime Monitor:** An n8n workflow that periodically checks a website's HTTP response and sends an email alert when the returned status is not `200` or the request fails.

## Folder Structure

Keep the exported workflow files and evidence in this structure before publishing the repository:

```text
Rohan_Salunkhe_Assignment/
├── README.md
├── Task1_QA_Report/
│   └── Task1_QA_Report_Rohan_Salunkhe.pdf
├── Task2_n8n_Workflow/
│   ├── Task2_Workflow_Rohan_Salunkhe.json
│   └── README_Task2.md
├── Bonus_Uptime_Monitor/
│   ├── Bonus_UptimeMonitor_Rohan_Salunkhe.json
│   └── README_Bonus_UptimeMonitor.md
└── Screenshots/
    ├── Task2_Workflow_Canvas.png
    ├── Task2_Successful_Execution.png
    ├── Task2_Error_Handling.png
    ├── Bonus_Healthy_Check_200.png
    └── Bonus_Down_Alert_503.png
```

The filenames above are suggested names. Add the actual exported JSON files, report, and screenshots to the matching folders before submission.

## Task 1 – Website QA

The Task 1 deliverable is the QA report PDF. It should be included in `Task1_QA_Report/` and contain the testing observations, reported issues, and root-cause analysis from the assessment.

## Task 2 – Daily Morning Brief Workflow

### Workflow logic

1. **Webhook Trigger** starts the workflow when it receives an HTTP request.
2. **Get All Posts** requests posts from `https://jsonplaceholder.typicode.com/posts`.
3. **Select Top 5 Posts** filters valid records, sorts them by post ID, and selects the first five.
4. **Get Author Details** requests the user record for each selected post from `https://jsonplaceholder.typicode.com/users/{userId}`.
5. **Combine Posts and Authors** attaches author name, username, and email details to each post.
6. **IF – Title Longer Than 50 Characters** splits the records based on whether the title length is greater than 50 characters.
7. The **True** branch formats and emails the long-title digest.
8. The **False** branch formats and emails the standard digest.
9. Errors from the two HTTP Request nodes are routed to **Format API Error Alert** and **Send API Failure Alert Email**.

### Credentials and configuration

- Configure SMTP credentials in n8n Credentials; do not place passwords or app passwords in the workflow JSON or this repository.
- Replace sender and recipient placeholders in all email nodes with the intended email addresses.
- Keep the posts endpoint set to `https://jsonplaceholder.typicode.com/posts` after any negative-path testing.
- The webhook is event-driven; it starts when its webhook URL is called. It is not a time-based schedule trigger.

### Reported verification

The workflow was run in n8n. The candidate reported successful receipt of the long-title digest, standard digest, and API-failure alert emails. The API-error branch was tested separately. Capture and include the actual n8n execution screenshots in `Screenshots/` as evidence.

**Implementation note:** The supplied Code node selects the first five valid posts by ID. It does not select five unique authors. For the standard JSONPlaceholder dataset, the first five posts share the same `userId`, so the author lookup may request the same author more than once.

## Bonus – Uptime Monitor

### Workflow logic

1. A Schedule Trigger is configured for a five-minute interval.
2. The HTTP Request node checks the monitored website using a GET request.
3. The response is configured to include the HTTP status and response headers.
4. The status-check logic treats HTTP `200` as healthy.
5. A non-`200` status is routed to an alert-formatting node and the uptime alert email node.
6. A request-level error is routed through the request-failure alert path, where configured.
7. A healthy response is recorded through the healthy-check path without sending a downtime alert.

### Configuration

- Monitored site: `https://demo.realworld.show/`
- Expected healthy status: `200`
- Schedule: every five minutes (the workflow must be saved and published/active for scheduled runs).
- Configure the alert email node with an n8n SMTP credential and the correct sender and recipient addresses.
- Do not commit SMTP credentials, app passwords, or other secrets to GitHub.

### Reported verification

The candidate reported that:

- The monitored site returned HTTP `200` and the healthy branch executed.
- A temporary HTTP `503` test caused the failure-alert branch to execute and an alert email was received.
- The monitored URL was then restored to `https://demo.realworld.show/` and the site returned HTTP `200` again.

Include the corresponding actual execution screenshots in `Screenshots/` before final submission.

## Importing the Workflows in n8n

1. Sign in to the n8n workspace.
2. Open **Workflows** and choose **Import from File** (or the equivalent import option in the editor menu).
3. Import the relevant `.json` workflow export.
4. Add/select the required SMTP credential in the email nodes.
5. Review sender and recipient fields and any environment-specific URLs.
6. Save the workflow. For the uptime monitor, publish/activate it so the Schedule Trigger can run automatically.
7. Run the relevant test cases and retain the execution evidence.

## Security Notes

- Never upload Gmail app passwords, SMTP passwords, API keys, or other credentials.
- Use n8n's credential store for secrets.
- Review exported workflow JSON before publishing; remove any private URLs, personal email addresses, or sensitive execution data that should not be public.
- Use test endpoints only for deliberate negative-path testing, then restore the production/demo monitoring URL.

## Final Submission Checklist

- [ ] Add the Task 1 QA report PDF.
- [ ] Add the final Task 2 workflow JSON export.
- [ ] Add the Task 2 README.
- [ ] Add the final Bonus Uptime Monitor JSON export.
- [ ] Add the Bonus README.
- [ ] Add genuine screenshots of the Task 2 workflow canvas and successful execution.
- [ ] Add genuine screenshots showing the Task 2 API-error alert test.
- [ ] Add genuine screenshots showing the Bonus HTTP `200` healthy check and HTTP `503` alert test.
- [ ] Confirm the Bonus workflow is restored to `https://demo.realworld.show/` and is published/active if scheduled monitoring is required.
- [ ] Remove credentials and secrets from all files before uploading to GitHub.

---

**Important:** This README documents the workflow design and the tests reported during the build. It does not replace the actual n8n workflow exports, QA report, or execution screenshots required by the assessment.
