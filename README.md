# SOAR-EDR-Project

## Objective
The objective of the SOAR-EDR project is to establish a robust Security Orchestration, Automation, and Response (SOAR) system integrated with Endpoint Detection and Response (EDR) using Tines, LimaCharlie, Slack and Gmail. This project aims to automate incident response workflows, enhance cybersecurity defenses, and validate detection rules through comprehensive testing and integration.

### Skills Learned

- Endpoint Detection and Response (EDR) configuration.
- SOAR playbook design and automation.
- Event monitoring and rule-based threat detection.
- Incident response using automated workflows.

### Playbook Workflow
1- Detection Retrieval: A LimaCharlie webhook detects a suspecious event
2- Alerting: Details such as Title, Username, source IP are sent to Slack and Email
3- Decision Prompt: Admins receive a Slack prompt asking if the affected machine should be isolated
    -A NO response will send a Slack Message indicating no action taken
    -A YES response wil execute LimaCharlie isolation and send the status update to Slack

