---
name: platform-engineer
description: Platform Engineer Skill
---

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Architecture & Security (The Blueprint)

**BEFORE writing a single line of YAML or Terraform:**

1.  **Define the Topology**
    - Draw the network diagram (VPCs, Subnets, Firewalls).
    - Where does the data live? Is it public or private?
    - **Rule:** nothing talks to the internet unless explicitly required. Default to private.

2.  **Identity & Access Management (IAM)**
    - Define "Who" needs access to "What."
    - **Principle of Least Privilege:** A build service should not have Admin access.
    - **No Hardcoded Secrets:** Plan where keys/passwords live (Secret Manager, Vault).

3.  **Cost Estimation**
    - Check the pricing calculator.
    - Set up Budget Alerts *before* deploying resources.
    - Avoid "Cloud Shock" by defining retention policies (logs, backups) now.

### Phase 1.5: FinOps & Cost Optimization (2026)

**Cloud costs are the #2 startup killer (after running out of customers):**

1.  **Tagging Strategy (Cost Accountability)**
    - **Required Tags:** environment, team, project, cost-center
    - **Enable:** Cost allocation reports, blame tracking
    - **Enforcement:** Tag policies (reject untagged resources)
    ```terraform
    resource "google_storage_bucket" "data" {
      labels = {
        environment = var.environment
        team        = "data-platform"
        project     = "analytics"
      }
    }
    ```

2.  **Right-Sizing & Committed Use**
    - **Spot/Preemptible Instances:** 60-90% cheaper for fault-tolerant workloads
    - **Reserved Instances:** 1-3 year commit for predictable loads (40-60% savings)
    - **Savings Plans:** Flexible commitment (AWS/Azure)
    - **Auto-scaling:** Scale down aggressively (nights, weekends)
    - **Tool:** Infracost (estimate before deploy), Kubecost (k8s)

3.  **Cost Alerts & Anomaly Detection**
    - **Budget Alerts:** 50%, 80%, 100%, 120% thresholds
    - **Per-Service Budgets:** Separate for Compute, Storage, Network
    - **Anomaly Detection:** Alert on 2x daily average
    - **Example:** "BigQuery cost is 10x higher today vs yesterday"

4.  **Storage Lifecycle Policies**
    - **Hot → Warm → Cold → Archive**
      - S3: Standard → IA → Glacier → Deep Archive
      - GCS: Standard → Nearline → Coldline → Archive
    - **Auto-delete:** Logs after 90 days, temp files after 7 days
    - **Savings:** 50-90% on storage costs

### Phase 2: Infrastructure as Code (IaC)

**Treat infrastructure like software:**

1.  **Codify Resources (Terraform/OpenTofu)**
    - **Rule:** Never click in the Console to create production resources ("ClickOps").
    - Use Modules for repeatable patterns (e.g., a standard bucket configuration).
    - Store state remotely and lock it (e.g., GCS bucket with state locking).

2.  **Immutability Strategy**
    - Don't SSH into servers to patch them.
    - Plan to replace infrastructure, not repair it ("Cattle, not Pets").
    - Define your container strategy (Dockerfiles, Base Images).

3.  **Environment Parity**
    - Staging should look like Production (just smaller).
    - Use variables to handle differences, not different logic.
    - **Goal:** If it works in Staging, it works in Prod.

### Phase 2.5: GitOps & Modern Deployment

**The infrastructure deployment evolution:**

1.  **GitOps Pattern**
    - **Git as Source of Truth:** Desired state lives in Git
    - **Pull-Based Deployment:** Cluster pulls config (vs push)
    - **Auto-Sync:** Changes to Git automatically deploy
    - **Tools:** ArgoCD (k8s), Flux, Atlantis (Terraform)
    - **Benefits:** Audit trail, easy rollback, no CI/CD credentials in cluster

2.  **Drift Detection**
    - **Problem:** Someone clicks in console, state diverges from Git
    - **Solution:** Automated drift detection + alerts
    - **terraform plan** in CI/CD on schedule
    - **ArgoCD:** Visual diff of desired vs actual state

3.  **Progressive Delivery**
    - **Blue/Green:** Deploy new version, switch traffic instantly
    - **Canary:** Route 5% → 25% → 100% traffic gradually
    - **Feature Flags:** Enable features for subset of users
    - **Tools:** Flagger (k8s), LaunchDarkly, Split.io

### Phase 3: The Pipeline (CI/CD)

**Automate the factory:**

1.  **Continuous Integration (CI)**
    - Lint the code (TFSec, Hadolint).
    - Run unit tests.
    - Build the artifact (Container Image, Binary).
    - **Rule:** The build fails if the tests fail. No exceptions.

2.  **Continuous Deployment (CD)**
    - Automate the rollout.
    - **Gatekeeping:** Require approval for Prod deployment?
    - **Strategy:** Blue/Green? Canary? Rolling Update? Choose one to minimize downtime.

3.  **Rollback Strategy**
    - "Break Glass" procedure: How do we undo the last deploy instantly?
    - Test the rollback mechanism. A rollback that fails is an outage.

### Phase 4: Observability & Reliability (SRE)

**Flying the plane:**

1.  **Logging & Tracing**
    - Centralize logs (don't leave them on the instance).
    - Ensure logs are structured (JSON) so they can be queried.
    - Correlate logs across services (Trace ID).

2.  **Metrics & Alerting**
    - **The Golden Signals:** Latency, Traffic, Errors, Saturation.
    - Alert on *symptoms* (User sees 500 error), not just *causes* (CPU high).
    - **Rule:** If an alert wakes you up, it must be actionable. If not, delete the alert.

3.  **Disaster Recovery (DR)**
    - Automate backups.
    - **Crucial:** Test the Restore. A backup you haven't restored is just a file you hope works.

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "I'll just change this firewall rule in the console quickly to test."
- "I'll hardcode the API key just for this commit."
- "I'll write the Terraform import later."
- "It works on my laptop, just ship the container."
- "We don't need logs for this service."
- "I'll ssh in and restart the service manually."
- **Using `admin` or `owner` permissions for a service account.**

**ALL of these mean: STOP. Return to Phase 1/2.**

## Your Human Partner's Signals You're Doing It Wrong

**Watch for these complaints:**
- **Dev:** "The build takes 20 minutes." (Your pipeline is bloated).
- **PM:** "Why did Staging break?" (You lacked Environment Parity).
- **Sec:** "Why is port 22 open to the world?" (You failed Phase 1).
- **Fin:** "Why is the bill 3x higher this month?" (You failed Cost Estimation).
- **Dev:** "I can't debug this without SSH." (You failed Observability).

**When you see these:** STOP. Fix the platform/process.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "ClickOps is faster for prototyping" | It creates "Configuration Drift" instantly. |
| "We'll secure the bucket later" | You will be hacked/scraped before "later" comes. |
| "I don't need a budget alert" | One infinite loop can cost $10k overnight. |
| "Terraform is too complex for this" | Manual maintenance is infinitely more complex over time. |
| "I'll document the manual steps" | Docs get outdated. Code (IaC) is the documentation. |
| "Logs are too expensive" | Downtime is more expensive. Sample/filter them. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Architecture** | Topology, IAM, Cost | Secure design, Budget set |
| **2. IaC** | Terraform, Docker, State | Reproducible infra, No manual clicks |
| **3. Pipeline** | Lint, Build, Test, Deploy | Automated flow from Git to Prod |
| **4. Observability** | Golden Signals, Logs, Backups | You know it's broken before users do |

## When Process Reveals "Legacy Drift"

When you inherit a project built via "ClickOps" (manual console work):

1.  **Don't nuke it.**
2.  **Audit:** Use tools (like `terraformer` or `gcp-export`) to read the current state.
3.  **Import:** Systematically import resources into Terraform state.
4.  **Lock Down:** Once imported, remove write access to the Console to force usage of the pipeline.

## Supporting Techniques

- **`superpowers:infrastructure-as-code`** - Writing clean Terraform modules.
- **`superpowers:container-security`** - Scanning images for vulnerabilities.
- **`superpowers:post-mortem`** - Learning from outages (Blameless RCA).

## Real-World Impact

- **"SysAdmin" Approach:** Manual servers, "it works on my machine," fear of Friday deploys, 3 AM wake-up calls.
- **"DevOps" Approach:** Immutable infrastructure, automated pipelines, deploys on Friday at 4 PM without sweating, sleeping through the night.

## 🛠️ Modern Platform Stack (2026)

### Infrastructure as Code
- **Terraform/OpenTofu:** Multi-cloud IaC
- **Pulumi:** Code-native IaC (TypeScript, Python, Go)
- **CDK (AWS/GCP):** Cloud-native IaC

### Container Orchestration
- **Kubernetes:** Industry standard (GKE, EKS, AKS)
- **Nomad:** Simpler alternative (HashiCorp)
- **Cloud Run / Fargate:** Serverless containers

### GitOps
- **ArgoCD:** Kubernetes deployments
- **Flux:** CNCF GitOps
- **Atlantis:** Terraform pull requests

### Serverless
- **Functions:** AWS Lambda, Cloud Functions, Cloudflare Workers
- **Edge Compute:** Cloudflare Workers (ms latency globally)
- **Containers:** Cloud Run, Fargate, Fly.io

### Observability
- **OpenTelemetry:** Unified standard
- **Platforms:** Datadog, Grafana Cloud, New Relic
- **Logs:** Loki, CloudWatch, Mezmo
- **Metrics:** Prometheus, VictoriaMetrics

### CI/CD
- **GitHub Actions:** Popular, integrated
- **GitLab CI:** Full DevOps platform
- **CircleCI / Buildkite:** Specialized

## 💰 FinOps Best Practices

### Cost Optimization Checklist
- [ ] Tag all resources with team/project/environment
- [ ] Set up budget alerts at 50%/80%/100%
- [ ] Use spot instances for dev/test environments
- [ ] Implement auto-scaling with aggressive scale-down
- [ ] Set lifecycle policies on storage (hot → cold → archive)
- [ ] Review and delete unused resources monthly
- [ ] Use reserved instances/savings plans for stable workloads
- [ ] Monitor egress costs (data transfer between regions)
- [ ] Compress and deduplicate backups
- [ ] Use CDN to reduce origin server traffic

### Cost Monitoring Dashboard
| Metric | Alert Threshold | Why |
|--------|----------------|-----|
| **Daily spend** | 2x 7-day average | Detect runaway costs |
| **Per-service budget** | 80% of monthly | Prevent overruns |
| **Egress traffic** | 10% increase | Network costs spike |
| **Storage growth** | 20% month-over-month | Lifecycle policies failing |
| **Idle resources** | > 7 days unused | Wasted spend |
