# Amber Deneal

**Project Manager, Business Transformation — Verizon Wireline Operations**
Moving into cloud and AI engineering, and building in public while I do it.

Thanks for stopping by. Here's the short version: I learn by doing. Every
certification on my study plan has a real build attached to it, so what's in these
repos is live infrastructure running in a real account with a real bill — not
tutorial exercises. I'm partway through a 33-week, seven-certification program
across AWS and Google Cloud, with two applications going up alongside it.

If you're here from a résumé or a conversation, the Weather AI section below is
probably what you're looking for.

---

## What I'm building

### Weather AI — [`weather-ai-app`](https://github.com/amberdeneal-builds/weather-ai-app) · public

**Live now:** `https://api.amberdeneal.dev/?lat=39.2904&lon=-76.6122&city=Baltimore`

**The problem.** When an application's core feature depends on a single cloud's AI
service, that's a single point of failure with no way to route around it. Plenty of
architectures diagram a failover. Far fewer have actually watched one work.

**What I built.** A serverless weather API that generates its AI insight from Amazon
Bedrock and quietly fails over to Google Vertex AI when Bedrock isn't available. The
caller gets a normal response either way and never knows which cloud answered.

**How it's put together.** API Gateway → Lambda (Python) → DynamoDB cache, pulling
live conditions from the National Weather Service and the written insight from
Bedrock's Claude Haiku 4.5, with Vertex AI Gemini 2.5 Flash as the backup. All of it
defined in Terraform — including the API Gateway and custom domain, which I brought
under management by importing the real resources rather than recreating them, so
there was zero downtime on a live endpoint.

**What came out of it.**

- **Failover verified in both directions, not assumed.** I deliberately broke the
  Bedrock model ID, confirmed in CloudWatch that Vertex picked up the request and the
  API still returned a complete response, then reverted and confirmed Bedrock resumed.
- **Runs for pennies a month.** Measured, not estimated — the whole account's
  uncredited run-rate was about **$4/month**, and most of that was an unrelated test
  instance. Budget alerts on both clouds now catch anything that moves.
- **Deployer IAM consolidated from 10 managed policies to 3** after hitting AWS's hard
  per-user cap, with identical effective permissions and a clean `terraform plan`
  verified at every step.

The repo documents the *decisions*, not just the inventory — why the Lambda left its
VPC once it needed a third-party API, why a cross-Region Bedrock inference profile
needs two IAM statements instead of one, and why DNS ended up on Cloudflare instead
of Route 53. Including the calls that turned out to be wrong.

### Pacer AI — `pacer-ai-core` · private

*Built from lived experience with chronic pain.*

**The problem.** People managing chronic pain tend to pace by how they feel that
morning — which is exactly when their judgment is least reliable. A good day invites
catching up on everything, and the crash arrives days later, long after the choices
that caused it. Standard calendars quietly assume infinite physical elasticity, which
makes a full schedule a genuine hazard rather than just a busy week.

**What I'm building.** A pacing engine that models flare risk from environmental and
activity signals — barometric pressure shifts, activity thresholds, biometric trends —
and surfaces it as a simple Green / Yellow / Red tier with a daily energy budget. When
risk rises, it proposes concrete changes: shift the afternoon errands, add a rest
interval, trade a standing task for a seated one.

**It proposes; you decide.** Nothing gets written to a real calendar without an
explicit tap. An app that silently rearranges your commitments during a flare would be
solving its own problem, not yours — so the engine's job is to notice early and make
the case, not to take the wheel.

Grounded in established clinical pacing models — time-contingent rather than
pain-contingent activity limits — and deliberately scoped as a **wellness tool, not a
medical device.**

**How it's put together.** Cross-cloud by design — AWS for ingestion (DynamoDB
Streams → EventBridge), Google Cloud for analytics and ML (Pub/Sub → BigQuery →
Vertex AI), with Workload Identity Federation handling auth between them so no
long-lived credentials ever cross the boundary.

**Where it stands.** In active development. The repo is private, so the architecture
and approach are what I can share here — the clinical logic stays in-house.

---

## Certifications

**Earned**

- Google Cloud — Generative AI Leader
- AWS Certified AI Practitioner (AIF-C01)

**In progress** — a foundational credential on each cloud already, working toward
seven more:

| Certification | Cloud |
|---|---|
| AWS Certified Cloud Practitioner (CLF-C02) | AWS |
| AWS Certified Solutions Architect – Associate (SAA-C03) | AWS |
| AWS Certified Developer – Associate (DVA-C02) | AWS |
| Google Cloud Professional Cloud Architect | GCP |
| Google Cloud Professional Data Engineer | GCP |
| Google Cloud Professional Machine Learning Engineer | GCP |
| AWS Certified Generative AI Developer – Professional | AWS |

---

## Tech

| | |
|---|---|
| **AWS** | Lambda · API Gateway · DynamoDB · Bedrock · Secrets Manager · KMS · IAM · VPC · CloudWatch · Budgets · Cost Explorer · ACM |
| **Google Cloud** | Vertex AI · IAM & service accounts · Cloud Billing · BigQuery *(in progress)* · Pub/Sub *(in progress)* |
| **Infrastructure as code** | Terraform |
| **Languages** | Python · SQL · Bash |
| **Also** | Git / GitHub CLI · Cloudflare DNS · REST API design |

---

## How I work

Three habits that shaped everything above, and that I'd bring to a team:

**Debug from real output, never from a guess.** Every fix in these repos traces back
to an actual error message, log line, or CLI response. It's slower for about ten
minutes and much faster after that.

**Verify what you claim.** A failover that has never failed over is a diagram. A
budget alert that emails nobody is worse than no alert, because it feels like
coverage.

**Write down the why.** What you did is recoverable from the code. Why you chose it
over the alternative is the expensive part to reconstruct six months later — so
that's what the READMEs explain.

---

Always glad to talk cloud architecture, multi-cloud tradeoffs, or what it actually
takes to move from managing technical programs to building them. Have a great day!
