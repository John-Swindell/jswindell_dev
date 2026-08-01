---
title: "Making Internal Tools Production Ready"
date: 2026-08-01
description: "How I am building a simple AWS path for turning useful internal prototypes into applications that other people can actually use."
summary: "How I am building a simple AWS path for turning useful internal prototypes into applications that other people can actually use."
tags: ["Platform Engineering", "AWS", "Terraform", "GitHub Actions", "Internal Developer Platforms"]
---
{{< button link="https://jswindell.dev" >}}
Back to Home Page
{{< /button >}}

{{< button link="https://jswindell.dev/blogs" >}}
View More Blogs
{{< /button >}}

<br>
<br>
<br>

### The gap after the prototype

Coding agents have made it much easier for people to build small tools for their own work. Someone can explain a slow process and end up with a surprisingly good single-page application in an afternoon.

Then they want a coworker to use it. The app only runs on localhost, its data lives in the browser, or it needs access to a system that cannot safely be called from frontend code. The idea is useful, but getting it from a laptop into production still requires authentication, shared infrastructure, deployment, and a recovery plan.

I have been building an internal platform at the Florida Panthers for that last part.

### Keeping the platform small

This is not meant to host every kind of application. It is a paved path for smaller internal tools with a known shape. A central YAML manifest describes each app, the services it can use, and the groups allowed to access it.

Most frontends are built as static assets, stored in S3, and served through CloudFront on their own subdomain. When an app needs warehouse data or a controlled backend operation, it calls a small Lambda service instead of receiving broad database credentials.

That boundary is intentional. The platform is a good fit when an app needs a limited set of governed operations. If the manifest cannot describe the system cleanly, the project probably needs its own architecture.

### Identity and access

Microsoft Entra ID provides the company identity, while Cognito handles application sessions and group claims in AWS. Teams can govern access to their own tools without creating another login system or sharing credentials.

The same approach carries into deployment. GitHub Actions authenticates to AWS through OIDC, and each workflow receives only the IAM permissions it needs. Application authors can work through pull requests and never have to manage long-lived AWS keys.

### Making the safe path fast

Terraform creates the repeatable infrastructure around each app. The delivery workflows add formatting, validation, linting, type checks, secret scanning, and a clear rollback path.

I care a lot about keeping those checks fast. Internal tooling loses momentum when a tiny change spends 15 minutes moving through CI. The platform should make a responsible deployment feel like the shortest route, especially when someone is fixing a real business problem.

The result is less about any one AWS service than it is about ownership. People can keep using the tools that help them build quickly, while the platform handles the parts that need to stay consistent across the company.
