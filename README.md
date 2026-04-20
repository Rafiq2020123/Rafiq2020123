# Rafiq — Senior SDET / Quality Engineering Architect

I've spent the last nine-plus years building test automation infrastructure that engineering teams actually rely on — not the kind that lives in a side repo nobody touches after sprint one. Most of my career has been in healthcare and fintech, which means I've had to care about things like PHI masking, FHIR R4 contract validation, audit trails, and PCI-adjacent data flows long before they became interview talking points. I've also done meaningful work in SaaS multi-tenant environments and government systems where Section 508 compliance isn't optional. My day-to-day spans the full stack: Playwright and Selenium for UI, REST/GraphQL/gRPC API validation, k6 and JMeter for load testing, OWASP ZAP for DAST, axe-core for accessibility, and the AWS/Azure infrastructure underneath all of it. I don't just write tests — I design the systems that run them, store their results, and alert when something breaks at 2 AM.

## What I Build

I design multi-project Playwright architectures with storageState-based auth, sharded parallel execution, and Allure reporting pipelines that publish to S3 static sites — because a test that passes but can't show you _why_ it passed is useless in a regulated environment. I build consumer-driven contract testing systems using Pact and schema validation so API teams stop breaking each other on Friday afternoons. I own CI/CD quality gates end-to-end: GitHub Actions matrix builds, Jenkins shared libraries, environment promotion rules, and the CloudWatch dashboards that track whether any of it is actually working. When a team needs to prove HIPAA compliance or WCAG 2.1 AA conformance, I build the automated evidence collection — not a slide deck.

## Featured Repositories

| Repository                                                                                                 | What It Does                                                                                                               |
| ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| [playwright-enterprise-framework](https://github.com/Rafiq2020123/playwright-enterprise-framework)         | Playwright + TypeScript with multi-project auth, storageState reuse, Allure, Docker, S3 hosting, ECS runner — the flagship |
| [api-contract-testing-framework](https://github.com/Rafiq2020123/api-contract-testing-framework)           | REST & GraphQL API testing with Pact CDC contracts, schema validation, Pact Broker in Docker, Azure Blob reporting         |
| [performance-load-testing-suite](https://github.com/Rafiq2020123/performance-load-testing-suite)           | k6 + JMeter load tests with threshold enforcement, Grafana dashboards, CloudWatch custom metrics                           |
| [cicd-quality-engineering](https://github.com/Rafiq2020123/cicd-quality-engineering)                       | GitHub Actions + Jenkins pipeline patterns, environment promotion gates, S3/Azure Blob artifact management                 |
| [accessibility-compliance-framework](https://github.com/Rafiq2020123/accessibility-compliance-framework)   | axe-core + Playwright for WCAG 2.1 AA, Lighthouse CI, Section 508 audit reports published to S3                            |
| [fhir-healthcare-validation-suite](https://github.com/Rafiq2020123/fhir-healthcare-validation-suite)       | FHIR R4 API testing against HAPI FHIR, PHI masking, HIPAA-aware design, HL7 validation, Parameter Store secrets            |
| [security-dast-integration-framework](https://github.com/Rafiq2020123/security-dast-integration-framework) | OWASP ZAP automation in CI/CD, severity threshold gating, Snyk dependency scanning, Burp Suite workflow docs               |
| [e2e-multilayer-quality-framework](https://github.com/Rafiq2020123/e2e-multilayer-quality-framework)       | Full-stack UI + API + DB validation against OrangeHRM, Docker Compose orchestration, Azure ACI runner                      |

## Tech Stack

**UI Automation:** Playwright (TypeScript), Selenium WebDriver (Java/Python), Cypress, Appium, WebdriverIO
**API & Contract:** REST Assured, Supertest, Pact CDC, GraphQL introspection, gRPC/protobuf, Postman/Newman
**Performance & Load:** k6, Apache JMeter, Locust, Gatling
**Security:** OWASP ZAP, Burp Suite, Semgrep, Snyk, SonarQube
**Accessibility:** axe-core, Lighthouse CI, Pa11y, NVDA/VoiceOver manual validation
**CI/CD & Cloud:** GitHub Actions, Jenkins, GitLab CI, Azure DevOps | AWS (ECS, S3, CloudWatch, Lambda, Parameter Store) | Azure (ACI, Blob Storage, Key Vault, Monitor)
**Languages:** TypeScript, JavaScript, Python, Java, Bash
**Reporting:** Allure, Playwright Trace Viewer, Grafana + InfluxDB, Datadog APM, ReportPortal

## Currently Focused On

- Building AI-assisted test failure triage that clusters Allure failure signatures and routes them to owning teams via Slack — tired of manually reading stack traces across 400+ nightly tests.
- Shifting contract testing left with Pact in PR-level CI checks so broken provider changes get caught before they hit a shared environment, not after.
- Wiring test infrastructure observability into Datadog: execution duration trends, flake rates by tag, and infra cost per test run — because "the tests are slow" isn't actionable without data.

---

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=Rafiq2020123&show_icons=true&theme=dark&include_all_commits=true&count_private=true&show=reviews,discussions_started,prs_merged)

↓ Pinned repositories below — start with **playwright-enterprise-framework** for the most complete example of my work.

**Connect:** [LinkedIn](https://linkedin.com/in/Rafiq2020123) · rafiq.sdet@gmail.com
