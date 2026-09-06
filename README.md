# Amber Deneal

Project Manager on the Business Transformation team at Verizon Wireline Operations,
moving toward cloud and AI engineering.

I learn by shipping. Every certification on my study plan has a matching hands-on
build attached to it, so what's in these repos is real infrastructure running in a
real account — not tutorial exercises. Currently working through a 33-week,
seven-certification program across AWS and Google Cloud, building two applications
alongside it.

---

## What I'm building

### Weather AI — [`weather-ai-app`](https://github.com/amberdeneal-builds/weather-ai-app) · public

**Live:** `https://api.amberdeneal.dev/?lat=39.2904&lon=-76.6122&city=Baltimore`

**Problem.** An application whose core feature depends on one cloud's AI service has
a single point of failure it can't route around.

**Solution.** A serverless weather API that generates its AI insight from Amazon
Bedrock, and transparently fails over to Google Vertex AI when Bedrock is
unavailable. The caller sees a normal response either way.

**Architecture.** API Gateway → Lambda (Python) → DynamoDB cache, with National
Weather Service data and Bedrock Claude Haiku 4.5 for the insight, falling back to
Vertex AI Gemini 2.5 Flash. Fully defined in Terraform, including the API Gateway
and custom domain, which were imported from console-created resources rather than
recreated. Custom domain on Cloudflare DNS with an ACM certificate.

**Outcome.** Live and verified in both directions — the failover was proven by
deliberately breaking the Bedrock model ID and confirming in CloudWatch that Vertex
served the request, then reverting and confirming Bedrock resumed. Runs for pennies
a month, with budget alerts on both clouds.

The repo documents the decisions, not just the inventory: why the Lambda left its
VPC once it needed a third-party API, why a cross-Region Bedrock inference profile
requires two IAM statements instead of one, and why the deployer's IAM had to be
consolidated from ten managed policies down to three.

### Pacer AI — `pacer-ai-core` · private

**Problem.** People managing chronic conditions often pace by how they feel that
morning, which is exactly when their judgment is least reliable — and by the time a
flare is obvious, the choices that caused it are already days old.

**Solution.** A predictive pacing engine that models flare risk from environmental
and activity signals, surfaces it as a simple Green / Yellow / Red tier with a daily
energy budget, and explains its reasoning rather than just issuing a verdict.
Deliberately scoped as a wellness tool, not a medical device.

**Architecture.** Cross-cloud by design: AWS for ingestion (DynamoDB Streams →
EventBridge), Google Cloud for analytics and ML (Pub/Sub → BigQuery → Vertex AI),
with Workload Identity Federation handling authentication between them so no
long-lived credentials cross the boundary.

**Outcome.** In active development. The repo is private, so the architecture and
approach are what I can share publicly — the clinical logic isn't.

---

## Certifications

**Earned**

- Google Cloud — Generative AI Leader
- AWS Certified AI Practitioner (AIF-C01)

**In progress**

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
| **Google Cloud** | Vertex AI · IAM & service accounts · Cloud Billing · BigQuery · Pub/Sub *(in progress)* |
| **Infrastructure as code** | Terraform |
| **Languages** | Python · SQL · Bash |
| **Other** | Git / GitHub CLI · Cloudflare DNS · REST API design |

---

## How I work

Three habits the projects above were built on:

**Debug from real output, never from a guess.** Every fix in these repos traces back
to an actual error message, CloudWatch log line, or CLI response — not a plausible
theory about what went wrong.

**Verify the thing you claim.** A failover that has never failed over is a diagram.
A budget alert that emails nobody is worse than no alert, because it feels like
coverage.

**Write down why, not just what.** The reasoning behind a tradeoff is the part that's
expensive to reconstruct six months later — so the READMEs in these repos explain
decisions, including the ones that turned out to be wrong.
