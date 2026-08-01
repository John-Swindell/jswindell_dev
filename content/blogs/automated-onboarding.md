---
title: "Automated Onboarding Pipeline & Headless LMS"
date: 2026-02-01
image: "/images/projects/automated-onboarding-base-final.webp"
description: "A production-grade onboarding pipeline connecting Airtable, DocuSign, Google Workspace, and an automatically generated headless LMS."
summary: "A production-grade onboarding pipeline connecting Airtable, DocuSign, Google Workspace, and an automatically generated headless LMS."
tags: ["Python", "GitHub Actions", "AI", "Automation", "Airtable API", "DocuSign API", "JWT", "OpenAI API"]
---
{{< button link="https://jswindell.dev" >}}
Back to Home Page
{{< /button >}}

{{< button link="https://github.com/John-Swindell/automated-onboarding-pipeline" >}}
View Source Code
{{< /button >}}

{{< button link="https://jswindell.dev/blogs" >}}
View More Blogs
{{< /button >}}

<br>
<br>
<br>

### **The Challenge**
Scaling a technical internship cohort involves a massive amount of administrative overhead. For our Spring '26 cohort, managing the lifecycle of 30+ incoming engineers created a significant bottleneck. The manual process involved reconciling data from our ATS, sending individual DocuSign envelopes, manually creating onboarding Google Docs, and provisioning Drive folder permissions. We needed a 100% automated, zero-touch system to handle the lifecycle from Offer Queued to Day 1 Onboarding without human intervention.

### **The Solution**
I architected a modular automation pipeline using Python micro-services. The system utilizes Airtable as a relational state engine, orchestrating interactions between DocuSign, Google Drive, and Google Docs. The result is a Headless LMS (Learning Management System) that dynamically generates curriculum materials and provisions access in real-time as candidates sign their offers.

*(Click the diagram to view full resolution)*

[![Automation Architecture](/images/projects/tekly-onboarding-diagram.webp)](/images/projects/tekly-onboarding-diagram.webp)

### Orchestration & State Management

* **Architecture:** Built a lightweight Python controller (`automation_scheduler.py`) that manages sequential workflows. It is designed to be containerized using Docker and deployed as a persistent background service.
* **State Machine:** Implemented a one-way ratchet logic in the ETL layer to strictly manage lifecycle states (Queued to Sent to Signed to Onboarding). The system respects a strict status hierarchy, ensuring that bulk data imports from the ATS never overwrite manual progress or duplicate offers.

### The Offer Engine (Legal Automation)

* **JWT Integration:** Developed a robust DocuSign integration using JWT Authentication for headless server-to-server communication, eliminating manual logins.
* **Dynamic Envelopes:** The `offer_automation.py` service detects Queued candidates and dynamically generates composite PDF envelopes containing the Offer Letter and NDA. It injects variable data, such as start dates and roles, directly into the contract text tabs via API before sending to ensure legally accurate and personalized contracts at scale.

### Infrastructure as Code & IssueOps

* **Magic Link Provisioning:** Replaced manual invites with a Magic Link workflow. Interns trigger a GitHub Action by opening a specific Issue, which validates their status and automatically invites them to the private Organization, provisions a private repository from a template, and injects granular team permissions for least privilege access.
* **Role-Based Access:** The system acts as a security bridge to Google Drive. Once a candidate reaches the Onboarding phase, the system identifies their specific track, such as Machine Learning or Data Engineering, and programmatically provisions Viewer access only to the relevant curriculum folders.

### Dynamic Documentation Engine

* **Programmatic Generation:** Instead of copying static templates, I built a documentation generator (`onboarding_doc_generator.py`) that constructs Google Docs via the API.
* **Batch Requests:** The system uses `batchUpdate` requests to construct documents block-by-block by inserting headers, styling text, and injecting unique Submit Progress links that tie back to the specific candidate's record in Airtable.

### AI-Driven Knowledge Engineering

* **Curriculum Synthesis:** Leveraged the OpenAI API (GPT-4) to ingest unstructured internal documentation and synthesize it into standardized, role-specific learning tracks, deploying a structured curriculum in days rather than weeks.

### Outcome

The system successfully scaled to handle the Spring '26 cohort intake with zero additional headcount required. We achieved a fully automated flow where an intern can be offered, signed, and provisioned with a dedicated coding environment without a single manual email sent by the engineering team. This reduced administrative time by approximately 90%, allowing the core team to focus on mentorship rather than logistics.
