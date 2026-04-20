# Portfolio Repository Strategy

Eight repositories. Each one exists because it solves a distinct problem domain that comes up in senior SDET interviews. No overlap, no filler.

---

## 1. playwright-enterprise-framework — FLAGSHIP

**Difficulty:** Staff

**What it is:** A production-grade Playwright test framework in TypeScript that demonstrates every pattern a Staff SDET would own: multi-project configuration with separate `setup` projects for authentication, `storageState`-based session reuse so login doesn't run before every spec, a layered Page Object Model with component abstractions split from full-page objects, and custom fixtures that manage API-seeded test data lifecycle. Allure reporting is integrated with `@allure/playwright` — not just installed, but configured with environment properties, severity annotations (`@severity:critical`), epic/feature/story hierarchy, and screenshot/trace attachments on failure. Tests run in Docker via `docker-compose.yml` with a Selenium Grid option for cross-browser and a Playwright-native option for speed. The target application is saucedemo.com.

**Why an interviewer respects it:** This isn't a "hello world Playwright repo." The auth architecture alone — using `globalSetup` to authenticate once, serialize to `.auth/user.json`, and inject via `storageState` in `playwright.config.ts` projects — is something most candidates can't explain on a whiteboard. The fixture design shows someone who understands Playwright's dependency injection, not just `page.goto()`. Sharded CI execution with `--shard` and merged Allure results across shards is a real production problem that most tutorial repos ignore entirely.

**Primary stack:** Playwright, TypeScript, Allure, Docker, Docker Compose

**Cloud deployment (AWS):** GitHub Actions uploads Allure report to S3 as a static site with `aws s3 sync`. Test runner image pushed to ECR and executed in ECS Fargate for scheduled nightly runs. CloudWatch custom metric published via `aws cloudwatch put-metric-data` tracking pass rate, failure count, and execution duration. Secrets pulled from AWS Parameter Store at pipeline runtime using `aws ssm get-parameter --with-decryption`.

---

## 2. api-contract-testing-framework

**Difficulty:** Senior

**What it is:** A dual-language API testing framework covering REST (reqres.in) and GraphQL (public GraphQL APIs) with TypeScript as the primary language and a Python `pytest` module as a secondary implementation showing cross-language contract patterns. The core differentiator is Pact consumer-driven contract testing: consumer tests generate pact files, a Dockerized Pact Broker stores and versions them, and provider verification runs against the broker. JSON Schema validation using `ajv` catches structural drift that status-code-only assertions miss. Request/response logging is structured for debugging. The framework separates API clients from test logic — the `clients/` layer wraps `axios` with typed request/response interfaces, while `tests/` compose workflows from those clients.

**Why an interviewer respects it:** Most API test repos are just a `describe` block with `fetch()` calls. This one shows contract ownership: who publishes, who verifies, how versions map to git branches, and what happens when a contract breaks. The Pact Broker Docker setup proves the candidate has actually run this workflow, not just read about it. Schema validation with `ajv` configured for `additionalProperties: false` shows someone who understands API drift in microservices.

**Primary stack:** TypeScript (Supertest, Pact, ajv), Python (pytest, requests, pact-python), Docker (Pact Broker + PostgreSQL)

**Cloud deployment (Azure):** Pact verification results published as JSON to Azure Blob Storage. Azure DevOps pipeline triggers provider verification on PR merge. Secrets managed via Azure Key Vault references in pipeline YAML. Azure Monitor alerts configured on contract verification failures.

---

## 3. performance-load-testing-suite

**Difficulty:** Senior

**What it is:** Performance testing framework combining k6 (primary, JavaScript-based scripted scenarios) and JMeter (secondary, XML-based enterprise patterns) targeting httpbin.org and public demo endpoints. k6 scripts define realistic scenarios — ramp-up, sustained load, spike, soak — with `thresholds` enforcing p95 latency and error rate SLOs. JMeter test plans demonstrate distributed execution with remote engines. Results flow into Grafana dashboards via InfluxDB for k6 and JMeter Backend Listener. The repo includes a `docker-compose.yml` that stands up k6 + InfluxDB + Grafana locally with a single command so anyone reviewing can see dashboards immediately.

**Why an interviewer respects it:** Knowing how to run a load test is table stakes. This repo shows threshold-based quality gates: the CI pipeline fails if p95 response time exceeds a defined SLO. The Grafana dashboard isn't just "requests per second" — it tracks percentile distribution, error categorization by HTTP status, and virtual user concurrency over time. The distinction between load, stress, spike, and soak scenarios is documented with rationale for when you'd run each. JMeter is included because enterprise teams still use it, and showing both tools signals pragmatism over dogma.

**Primary stack:** k6 (JavaScript), JMeter, Grafana, InfluxDB, Docker Compose

**Cloud deployment (AWS):** k6 results published to CloudWatch custom metrics namespace `QualityEngineering/Performance` using a post-test script that calls `put-metric-data`. CloudWatch alarms trigger SNS notification when p95 exceeds threshold. JMeter distributed execution documented for EC2-based remote engines. Grafana dashboard JSON exported and versioned in repo.

---

## 4. cicd-quality-engineering

**Difficulty:** Staff

**What it is:** Not a test framework — this is a CI/CD pipeline engineering repository. It contains production-grade GitHub Actions workflows (matrix builds, reusable workflows with `workflow_call`, composite actions for common steps like "setup Node + install deps + run tests + upload artifacts"), a Jenkins declarative pipeline with shared library structure (`vars/`, `src/`, `resources/`), and documentation comparing both systems with honest trade-offs. Environment promotion logic gates test passage in `dev` before allowing deployment to `staging`. Test failure triage automation parses Allure results and posts summary to a Slack webhook. Artifact lifecycle management pushes reports to both S3 and Azure Blob with retention policies.

**Why an interviewer respects it:** Pipeline code is infrastructure. Most SDETs treat CI as a YAML file they copy from Stack Overflow. This repo shows reusable workflow design (DRY pipeline composition), matrix strategy for cross-browser/cross-environment execution, environment protection rules with required reviewers, and artifact management that isn't just "upload artifact." The Jenkins shared library proves the candidate can work in enterprise Java shops that haven't moved to GitHub Actions. The comparison doc shows someone who picks tools based on constraints, not trends.

**Primary stack:** GitHub Actions (YAML), Jenkins (Groovy), Docker, Bash

**Cloud deployment (AWS + Azure):** S3 bucket with lifecycle rules for Allure report hosting — objects expire after 90 days. Azure Blob Storage as secondary artifact target with SAS token–based access. CodeBuild integration documented for AWS-native pipeline option. Azure DevOps pipeline YAML included as third CI platform reference.

---

## 5. accessibility-compliance-framework

**Difficulty:** Senior

**What it is:** Automated accessibility testing framework using `@axe-core/playwright` as the engine, targeting healthcare.gov (a real government site with accessibility obligations). Tests assert WCAG 2.1 Level AA conformance: color contrast, ARIA label presence, keyboard navigability, heading hierarchy, and form label association. Lighthouse CI runs in the pipeline and tracks accessibility scores over time with a budget file that fails the build if the score drops below a threshold. Reports are structured for audit evidence: each violation includes the WCAG success criterion number, the DOM selector, and a remediation suggestion. The output format is designed so a compliance officer can read it without engineering help.

**Why an interviewer respects it:** Accessibility testing is the most underserved area in SDET portfolios. Government contracts and healthcare companies require Section 508 compliance, and most automation engineers have never touched it. This repo shows someone who understands `axe.run()` configuration — including `runOnly` with specific WCAG tags, `resultTypes` filtering, and how to exclude known third-party widget violations using `exclude` selectors. The audit report format — not just a JSON dump but a structured HTML/Markdown report — shows production thinking.

**Primary stack:** Playwright, TypeScript, axe-core, Lighthouse CI, Pa11y

**Cloud deployment (AWS):** Audit reports published to S3 as a static HTML site with CloudFront distribution for stakeholder access. Lighthouse CI results stored in S3 with historical trend tracking. Lambda function triggers weekly scheduled accessibility scans against staging environment. Parameter Store holds target URLs and configuration.

---

## 6. fhir-healthcare-validation-suite

**Difficulty:** Staff

**What it is:** API validation suite purpose-built for healthcare interoperability standards. Tests FHIR R4 resources (Patient, Observation, Encounter, MedicationRequest) against the public HAPI FHIR server at `hapi.fhir.org/baseR4`. Validates FHIR Bundle pagination, `_include` and `_revinclude` search parameter behavior, and OperationOutcome error structures. HL7v2 message parsing validates ADT (Admit/Discharge/Transfer) message segments. Every test artifact goes through a PHI masking utility that strips names, MRNs, dates of birth, and SSN patterns from logs, screenshots, and report attachments before they're written to disk. Audit trail verification checks that FHIR AuditEvent resources are created for sensitive operations.

**Why an interviewer respects it:** Healthcare tech companies — Epic, Cerner (Oracle Health), Athenahealth — need SDETs who understand FHIR and HL7, not just HTTP. This repo proves the candidate knows the difference between a FHIR `searchset` Bundle and a `transaction` Bundle, understands `_count` and `_offset` pagination, and can validate `reference` fields resolve correctly. The PHI masking layer is the kind of detail that separates someone who has worked in HIPAA-regulated environments from someone who just lists "healthcare" on their resume. No PHI ever appears in test output — that's not optional, it's a legal requirement.

**Primary stack:** TypeScript, HAPI FHIR client, Python (HL7 parsing with `python-hl7`), Jest/Vitest

**Cloud deployment (AWS):** All secrets (FHIR server credentials, test patient identifiers) stored in AWS Parameter Store with `SecureString` type. Test results written to S3 with server-side encryption (SSE-S3). CloudWatch alarms monitor FHIR endpoint availability. IAM roles follow least-privilege: the test runner role has `s3:PutObject` and `ssm:GetParameter` only — no admin access.

---

## 7. security-dast-integration-framework

**Difficulty:** Senior

**What it is:** Automated Dynamic Application Security Testing (DAST) framework integrating OWASP ZAP into CI/CD pipelines. ZAP runs against a locally deployed OWASP Juice Shop (via Docker Compose) — a deliberately vulnerable application that provides realistic findings without targeting production systems. The pipeline performs a ZAP baseline scan, then a full active scan, and parses the JSON report to enforce severity thresholds: any HIGH or CRITICAL finding fails the build. Snyk CLI runs as a separate pipeline step for dependency vulnerability scanning with `--severity-threshold=high`. Burp Suite integration is documented as a manual/semi-automated workflow with configuration export files, because Burp isn't free and CI integration requires Burp Enterprise.

**Why an interviewer respects it:** Security testing in SDET portfolios is rare and immediately signals a senior-level candidate. The threshold enforcement is key: it's not just "run ZAP and look at the HTML report." The pipeline parses ZAP's JSON output, counts findings by severity, and gates the build — the same pattern used in real security-conscious organizations. Documenting Burp Suite as a manual workflow instead of pretending it's automated shows honesty and practical experience. The Snyk integration demonstrates shift-left dependency scanning, which is increasingly expected in DevSecOps teams.

**Primary stack:** OWASP ZAP (Docker), Snyk CLI, Bash/Python (report parsing), Docker Compose (Juice Shop)

**Cloud deployment (AWS):** ZAP scan reports uploaded to S3 with versioning enabled for audit history. CloudWatch Events triggers scheduled weekly full scans via CodeBuild. SNS alerts on any new HIGH/CRITICAL findings. Snyk results also pushed to S3 for consolidated vulnerability tracking.

---

## 8. e2e-multilayer-quality-framework

**Difficulty:** Senior

**What it is:** End-to-end quality framework that validates across three layers — UI, API, and database — in a single test run. Targets the OrangeHRM demo application (`opensource-demo.orangehrmlive.com`) for UI flows, its underlying API for data verification, and a PostgreSQL database layer (running locally in Docker) for state validation. A typical test scenario: create an employee through the UI, verify the API returns the correct record, confirm the database row matches. This multi-layer assertion pattern catches bugs that single-layer tests miss: the UI might show "saved" while the API returns stale data because a cache wasn't invalidated. Docker Compose orchestrates the application, database, and test runner as a single environment.

**Why an interviewer respects it:** Most E2E repos test one layer and call it "end-to-end." This one validates the actual data flow through the system, which is what real E2E testing means. The database validation layer — using `pg` client with typed query helpers and teardown hooks that clean up test data — shows infrastructure maturity. Docker Compose environment orchestration means a reviewer can clone the repo, run `docker-compose up`, and execute the full suite without installing anything. That's the bar for a senior-level demo repo.

**Primary stack:** Playwright (TypeScript), Supertest, PostgreSQL (`pg` client), Docker Compose

**Cloud deployment (Azure):** Test runner containerized and deployed to Azure Container Instances (ACI) for on-demand execution. Test results and Allure reports stored in Azure Blob Storage. Azure Key Vault manages database credentials and API tokens. Azure Monitor tracks test execution metrics and alerts on failure rate spikes. Azure DevOps pipeline orchestrates the full multi-layer test run.

---

## Summary Matrix

| #   | Repository                          | Difficulty | Primary Cloud                 | Key Differentiator                           |
| --- | ----------------------------------- | ---------- | ----------------------------- | -------------------------------------------- |
| 1   | playwright-enterprise-framework     | Staff      | AWS (S3, ECS, CloudWatch)     | Multi-project auth, storageState, sharded CI |
| 2   | api-contract-testing-framework      | Senior     | Azure (Blob, Key Vault)       | Pact CDC with Dockerized broker              |
| 3   | performance-load-testing-suite      | Senior     | AWS (CloudWatch, SNS)         | k6 thresholds as CI quality gates            |
| 4   | cicd-quality-engineering            | Staff      | AWS + Azure                   | Reusable workflows, Jenkins shared libs      |
| 5   | accessibility-compliance-framework  | Senior     | AWS (S3, CloudFront, Lambda)  | WCAG 2.1 AA audit evidence generation        |
| 6   | fhir-healthcare-validation-suite    | Staff      | AWS (Parameter Store, S3 SSE) | FHIR R4 + HL7 + PHI masking                  |
| 7   | security-dast-integration-framework | Senior     | AWS (S3, CodeBuild, SNS)      | ZAP severity threshold CI gate               |
| 8   | e2e-multilayer-quality-framework    | Senior     | Azure (ACI, Blob, Key Vault)  | UI + API + DB three-layer validation         |
