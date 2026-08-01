---
title: "How I Automated Technical Onboarding"
date: 2026-02-01
image: "/images/projects/automated-onboarding-base-final.webp"
description: "How I connected Airtable, DocuSign, Google Workspace, and generated training materials into one onboarding flow."
summary: "How I connected Airtable, DocuSign, Google Workspace, and generated training materials into one onboarding flow."
source: "https://github.com/John-Swindell/automated-onboarding-pipeline"
tags: ["Python", "GitHub Actions", "AI", "Automation", "Airtable API", "DocuSign API", "JWT", "OpenAI API"]
---
### Why I built it

Onboarding more than 30 technical interns meant repeating the same work across Airtable, DocuSign, Google Docs, and Drive. Offers had to be assembled and sent, signatures had to be tracked, accounts needed the right access, and each person needed training material for their track.

None of those steps was especially difficult by itself. Keeping all of them in sync was the real problem. I built one Python pipeline to move a candidate from a queued offer through signing and into their first-day setup.

### How the flow works

Airtable holds the current state for each person. A small controller checks that state and runs only the next valid step. Status moves in one direction, so a later import cannot accidentally move someone backward or send a duplicate offer.

[![Automation Architecture](/images/projects/tekly-onboarding-diagram.webp)](/images/projects/tekly-onboarding-diagram.webp)

When an offer is ready, the pipeline creates a DocuSign envelope containing the offer letter and NDA. It fills in details such as the role and start date, then sends the envelope through a server-to-server JWT integration.

Once the offer is signed, the same record drives the rest of the setup. The system grants access only to the Drive folders for that person's learning track and generates a Google Doc with the right curriculum and progress links.

### Accounts and training

I also used GitHub Actions as a lightweight entry point for account setup. An intern opens a specific issue, the workflow checks their status, and then it invites them to the organization, creates a private repository from a template, and applies the right team permissions.

The training documents are generated through the Google Docs API instead of copied by hand. I used the OpenAI API to help turn scattered internal notes into a consistent first version of each learning track, then kept the final structure in code so it could be reviewed and regenerated.

### What changed

The pipeline handled the Spring 2026 cohort without adding more administrative work to the engineering team. It cut the time spent on onboarding by about 90 percent and, more importantly, made the process predictable. The team could focus on teaching while the routine setup happened in the background.
