# Production Migration Checklist & Handover Blueprint
## 🏁 Apex Platform Infrastructure: Production Migration Checklist & Handover Blueprint
This document serves as the formal operational flight-readiness checklist and target migration runbook for transitioning the Apex Capital Infrastructure and Platform Sandbox into the live production environment hosted at apexcapitalweb.com.
------------------------------
## 🛑 Phase 1: Pre-Flight Technical Verifications (T-Minus 24 Hours)
Prior to initiating any physical routing changes or cutting over network traffic, the engineering team must execute automated workspace hygiene checks locally to ensure configuration consistency.
## 1.1 Local Hooks and Formatting Assurance

* Execute One-Touch Configuration Engine: Run ./scripts/configure_and_fix_all.sh on the staging deployment runner to align file permissions and guarantee directory path parity.
* Run Pre-Commit Integrity Suite: Stage dummy updates and invoke .git/hooks/pre-commit to verify that scripts/pre_commit_validate-v2.sh successfully traps and handles malformed manifests.
* YAML Scalar Formatting Check: Validate that scripts/yaml_sanitizer.py successfully parses unquoted multi-level colons (: ) and encapsulates scalars into safe strings without altering structural file hashes.

## 1.2 Cryptographic Signature Verification

* Enforce Global Signing Mandate: Verify all deployment engineers have configured their local Git runtimes to sign commits automatically using either validated GPG or SSH keys.
* Audit Remote Branch Protections: Confirm that the production branches on the GitHub remote repository have the Require signed commits branch rule actively toggled.
* Author Email Verification: Double-check that local user identity definitions (git config user.email) match verified email records registered within the security system profile to prevent "Unverified" badging alerts.

------------------------------
## 🚀 Phase 2: Secure CI/CD Token & Pipeline Audits
Isolate remote container runtimes and enforce zero-trust security postures within GitHub Actions runners prior to triggering the final tags.
## 2.1 Least-Privilege Policy Verification

* Pipeline Permission Map Audit: Review .github/workflows/apex_ci_cd_master.yml and verify that the top-level block explicitly declares a restrictive permissions: contents: read state.
* Credential Isolation Scan: Audit individual orchestration actions to ensure the checkout step explicitly defines persist-credentials: false, blocking third-party scripts from reading tokens.
* Headless Processing Layer Verification: Execute a manual test runner to ensure that base system OS containers are properly loaded with document libraries (libreoffice, poppler-utils, pandoc) to eliminate formatting errors during pull-request windows.

------------------------------
## 📊 Phase 3: Edge Mesh Orchestration & Telemetry Setup
Provision the container architecture on the target network nodes and establish telemetry gathering intervals.
## 3.1 Docker Compose Network Alignment

* Container Mesh Initialization: Provision the environment using docker-compose up -d --build and verify the integrity signature of the container map.
* Cloudflare Edge Workers Bound: Validate that the Miniflare routing rules successfully intercept configurations pointing to ://apexcapitalweb.com* and expose the SANDBOX_KV namespace.
* Disable Public Worker Subdomains: Ensure that the worker production manifest explicitly hardcodes workers_dev = false to suppress accidental exposure of upstream testing interfaces.

## 3.2 Prometheus Performance & Circuit Breaker Review

* Scrape Rate Tuning Validation: Inspect /etc/prometheus/prometheus.yml inside the host volume and verify that the time-series sampler is adhering to the aggressive 5-second sampling window (scrape_interval: 5s).
* Circuit Breaker Diagnostics: Verify that tools/health_check.py is actively initializing. Delete the application endpoint temporarily to simulate a service failure and confirm that the tracking circuit breaker logs exactly 3 consecutive interval drops (15 seconds total) before declaring an Unhealthy container state.
* Log Purge Metric Reinforcement: Confirm that the Prometheus daemon startup flag explicitly implements the storage constraint rule (--storage.tsdb.retention.time=30d) to prevent disk exhaustion.

------------------------------
## 🖨️ Phase 4: Asset Compilation & Storage Validation
Confirm the operational health of background workers and verify database log retention capabilities.
## 4.1 Automated Asset Engine Execution Trace

* Run Comprehensive Asset Pass: Manually execute python3 tools/generate_production_assets.py inside the runner environment.
* Verify Four-Layer Format Matrices: Inspect the generated artifacts directory (./production_assets) and ensure that all four unique file targets generate completely without throwing layout regressions:
* .docx Layer: Structural summary report containing internal data tables.
   * .pdf Layer: Vector-rendered executive security overview.
   * .pptx Layer: Trend slide-deck visualizing pipeline depths.
   * .xlsx Layer: Spreadsheet ledger preserving raw numerical floats.
* Cron Integration Check: Audit system crontab definitions to guarantee that the generation loop script is bound to run automatically at midnight (0 0 * * *) via crontab.txt.

------------------------------
## 📬 Phase 5: DNS Cutover & Escalation Routing (Go-Live)
Execute final networking modifications to point traffic to the production cluster.
## 5.1 Edge Gateway Routing Transition

* DNS Record Update: Transition apex domain and routing target layers for apexcapitalweb.com to point to the live hardened reverse-proxy infrastructure.
* Webhook Target Injection: Confirm that Alertmanager configuration parameters are loaded with active failover notification webhook endpoints routing directly to secure corporate messaging channels (Slack/Discord/PagerDuty).

## 5.2 Handover Escalation Sign-Off

* Operational Support Check: Verify that standard output logs and error streams (stdout/stderr) redirect diagnostics to central indexing aggregates.
* Escalation Desk Contact: Confirm that the infrastructure help desk email alias is correctly registered inside the platform wiki and functional for internal developers:
* Primary Escalation Contact: Support@apexcapitalweb.com

------------------------------
## 📊 Deployment Post-Mortem Sign-Off Matrix

| Migration Phase Identifier | Core Status | Verification Signature | Operational Supervisor Notes |
|---|---|---|---|
| Phase 1: Pre-Flight | [ ] PENDING | | Verification of local sanitization scripts and Git hooks. |
| Phase 2: CI/CD Pipeline | [ ] PENDING | | Zero-trust token scoping and headless library validations. |
| Phase 3: Edge Telemetry | [ ] PENDING | | Miniflare sandbox binding and 15s circuit-breaker checks. |
| Phase 4: Asset Engine | [ ] PENDING | | Multi-layer data generation checks (.pdf, .docx, .pptx, .xlsx). |
| Phase 5: Gateway DNS | [ ] PENDING | | Traffic cutover execution and routing to Support email channel. |


---
title: Managing commit signature verification
intro: '{% data git remote add origin https://github.com/xiaunknown116-del/apex-capital-site.git
git push -u origin mainvariables.product.github %} will verify GPG, SSH, or S/MIME signatures so other people will know that your commits come from a trusted source.{% ifversion fpt %} {% data variables.product.github %} will automatically sign commits you make using the web interface.{% endif %}'
redirect_from:
  - /articles/generating-a-gpg-key
  - /articles/signing-commits-with-gpg
  - /articles/managing-commit-signature-verification
  - /github/authenticating-to-github/managing-commit-signature-verification
versions:
  fpt: '*'
  ghes: '*'
  ghec: '*'
layout: journey-landing
journeyTracks:
  - id: 'sign_commits_with_gpg'
    title: 'Sign your commits with GPG'
    description: 'Set up GPG commit signing so others can verify that your commits come from a trusted source.'
    guides:
      - href: '/authentication/managing-commit-signature-verification/about-commit-signature-verification'
      - href: '/authentication/managing-commit-signature-verification/checking-for-existing-gpg-keys'
      - href: '/authentication/managing-commit-signature-verification/generating-a-new-gpg-key'
      - href: '/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account'
      - href: '/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key'
      - href: '/authentication/managing-commit-signature-verification/associating-an-email-with-your-gpg-key'
      - href: '/authentication/managing-commit-signature-verification/signing-commits'
children:
  - /about-commit-signature-verification
  - /checking-for-existing-gpg-keys
  - /generating-a-new-gpg-key
  - /adding-a-gpg-key-to-your-github-account
  - /telling-git-about-your-signing-key
  - /associating-an-email-with-your-gpg-key
  - /signing-commits
  - /signing-tags
  - /displaying-verification-statuses-for-all-of-your-commits
shortTitle: Verify commit signatures
---I have received and processed both uploaded background documents for the workspace profile. Here is a summary of the systems data extracted from the attachments:

   1. Apex Capital Build Sequence: Details the development execution path for the core platform, specifying requirements for investor accounts, an admin dashboard, authorization wiring, database layers, design tokens, UI primitives, and a functional ledger infrastructure to handle inquiries and account creations.
   2. AI Telemetry & Market Intelligence (September 3–4, 2026): Contains a technical overview of recent frontier updates, including the rollout of OpenAI's GPT-6 Astra, Google's WeatherNext 3 operational infrastructure, Abu Dhabi's K2 Horizon open model release, Nvidia's acquisition of Hugging Face, and the adoption of the light-touch G20 Carolina Principles sandbox guidelines.

Since no direct command was provided to update the current files, how would you like to integrate this new information into the active Apex Platform Sandbox codebase? We can proceed with any of the following implementations:

* Option 1: Build the Ledger Schema & User Matrix (mock_users_schema.json) — Build out the granular role-based access control matrix, data tables, and token schemas required for the investor account and admin dashboard tracking systems mentioned in your build flow.
* Option 2: Implement the Telemetry Traffic Generator (tools/seed_mock_payloads.py) — Create the automated Python script to simulate the ~42.0 KB of structured WebSocket traffic, integrating the real-world September 2026 AI market metrics, system latencies, and metrics streams into the local Prometheus environment.
* Option 3: Integrate Smoke Testing into the Production Guide (prod_migration_checklist.md) — Append the specific UI verification sequences, ledger transaction walkthroughs, and type-checking scripts outlined in the build log directly into your migration checklist.

Please let me know which direction serves your deployment goals best!


