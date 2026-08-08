# MODE — Market Opportunity Discovery Engine

> **An evidence-driven, configurable intelligence engine for discovering where commercial opportunities emerge, identifying organizations likely to experience them, and prioritizing the actions most likely to create value.**

[![Status](https://img.shields.io/badge/status-MVP-orange.svg)](#project-status)
[![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)](#requirements)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

## Abstract

Most sales-intelligence systems begin with a predefined market, customer profile, or contact database. **MODE** addresses the upstream problem: determining **where a commercially relevant problem is emerging and which organizations are most likely to need a given offer**.

MODE models the relationship between an offer, the problems it addresses, observable market signals, organizations, evidence, opportunity timing, potential buyers, and commercial outcomes. The system is designed to be configurable rather than hard-coded, allowing the same intelligence pipeline to be applied to different products, services, industries, and geographies.

The central objective is not maximum lead volume. It is **evidence-backed opportunity prioritization**.

```text
Offer
  ↓
Problem
  ↓
Market hypotheses
  ↓
Observable signals
  ↓
Organizations
  ↓
Evidence
  ↓
Opportunity assessment
  ↓
Buyer identification
  ↓
Recommended action
  ↓
Commercial outcome
  ↓
Model feedback
```

---

## 1. Motivation

Conventional lead-generation workflows commonly follow:

```text
Known market
    ↓
Known ICP
    ↓
Find companies
    ↓
Find contacts
    ↓
Enrich records
    ↓
Outreach
```

This workflow is effective when the target market and buying conditions are already understood.

MODE instead targets the preceding uncertainty:

```text
What can I provide?
        ↓
What problems does it solve?
        ↓
Where do those problems occur?
        ↓
What observable signals indicate them?
        ↓
Which organizations exhibit those signals?
        ↓
Which opportunities are active now?
```

This makes MODE an **opportunity-discovery layer**, rather than another static contact database.

---

## 2. Design Objectives

MODE is guided by the following objectives:

1. **Opportunity before contact** — identify a commercial opportunity before selecting a person to contact.
2. **Evidence before inference** — preserve the observations supporting each recommendation.
3. **Timing awareness** — distinguish long-term company fit from current buying opportunity.
4. **Negative intelligence** — model evidence that an organization should not be prioritized.
5. **Configuration over hard-coding** — allow market definitions and scoring strategies to change without rewriting the core system.
6. **Provider independence** — abstract external data sources from the intelligence layer.
7. **Explainability** — make recommendations inspectable and reproducible.
8. **Closed-loop learning** — use commercial outcomes to improve future opportunity identification.
9. **Data minimization** — collect only information necessary for the intended analytical purpose.
10. **Reproducibility** — preserve configuration, evidence provenance, timestamps, and scoring inputs wherever practical.

---

## 3. System Architecture

```text
                         ┌────────────────────┐
                         │       OFFER        │
                         │ Product / Service  │
                         │    Capability      │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │    OFFER MODEL     │
                         │                    │
                         │ Problems solved    │
                         │ Capabilities       │
                         │ Constraints        │
                         │ Buyer hypotheses   │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │    MARKET MODEL    │
                         │                    │
                         │ ICP hypotheses     │
                         │ Segments           │
                         │ Geography          │
                         │ Exclusions         │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │ SIGNAL DISCOVERY   │
                         │                    │
                         │ Hiring             │
                         │ Growth             │
                         │ Technology         │
                         │ Funding            │
                         │ Regulation         │
                         │ Public problems    │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │   EVIDENCE GRAPH   │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │ OPPORTUNITY ENGINE │
                         └─────────┬──────────┘
                                   │
                    ┌──────────────┼──────────────┐
                    ▼              ▼              ▼
                Market Fit    Intent Score   Timing Score
                    │              │              │
                    └──────────────┼──────────────┘
                                   ▼
                         ┌────────────────────┐
                         │   PRIORITIZATION   │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │ RECOMMENDED ACTION │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │      OUTCOME       │
                         └─────────┬──────────┘
                                   │
                                   ▼
                         ┌────────────────────┐
                         │ FEEDBACK / LEARN   │
                         └────────────────────┘
```

---

## 4. Core Concepts

### 4.1 Offer

An **offer** is the product, service, capability, or solution that the user can provide.

An offer contains:

- Description
- Capabilities
- Problems addressed
- Constraints
- Expected outcomes
- Potential buyer hypotheses

Example:

```yaml
offer:
  name: "Open-source security assessment"

  description: >
    Automated security assessment and remediation
    support for software projects.

  solves:
    - software_supply_chain_risk
    - dependency_risk
    - security_compliance

  capabilities:
    - repository_analysis
    - vulnerability_detection
    - security_posture_assessment
    - remediation_guidance
```

---

### 4.2 Market Model

The **market model** represents the organizations and environments in which an offer may be valuable.

MODE should permit both explicit criteria and hypotheses.

```yaml
market:
  industries:
    - software
    - infrastructure
    - cybersecurity

  company_size:
    min: 20
    max: 5000

  geographies:
    - Europe
    - North America

  hypotheses:
    - maintains_significant_open_source_software
    - operates_security_sensitive_infrastructure
    - has_a_growing_engineering_organization
```

The distinction is important: a user may know some characteristics of a target market while being uncertain about others. MODE should be able to test those assumptions rather than treating them as permanent truth.

---

### 4.3 Signals

A **signal** is an observable event, characteristic, or change that may indicate a relevant business condition.

Potential signal classes include:

| Class | Examples |
|---|---|
| Growth | Hiring acceleration, expansion, new facilities |
| Technology | Adoption, migration, infrastructure changes |
| Organization | New leadership, team formation, specialized hiring |
| Financial | Funding, acquisition, investment |
| Regulatory | Compliance obligations, regulatory deadlines |
| Product | Launches, major releases, new capabilities |
| Problem | Publicly described technical or operational problems |
| Market | New competitors, partnerships, procurement activity |

Signals should be configurable and weighted according to the use case.

---

### 4.4 Evidence

An **evidence item** is an observation supporting or contradicting an opportunity hypothesis.

Each evidence item should preserve provenance where available:

```json
{
  "type": "hiring_signal",
  "observation": "Company is hiring security engineers",
  "source": "authorized_source",
  "observed_at": "2026-08-09T00:00:00Z",
  "confidence": 0.94
}
```

MODE should distinguish between:

```text
OBSERVED
    ↓
VERIFIED
    ↓
INFERRED
    ↓
UNKNOWN
```

For example:

```text
Observed:
The organization posted five security engineering positions.

Inference:
The organization may be expanding its security capabilities.

Unsupported conclusion:
The organization is currently purchasing security software.
```

The final statement must not be represented as fact without independent evidence.

---

### 4.5 Opportunity

An **opportunity** is a potential commercial match between an organization, a problem, an offer, and a relevant time window.

Conceptually:

```text
Opportunity =
    Organization
  + Problem
  + Offer
  + Timing
  + Evidence
  + Potential Buyer
```

Example:

```json
{
  "organization": "Example Corp",
  "offer": "Security Assessment",
  "problem": "Software supply-chain risk",
  "fit_score": 91,
  "intent_score": 84,
  "timing_score": 89,
  "evidence_confidence": 0.93,
  "priority": "high"
}
```

---

## 5. Opportunity Scoring

MODE should use transparent scoring components rather than an unexplained single value.

A conceptual scoring function is:

```text
Opportunity Score =
    Market Fit
  + Problem Fit
  + Solution Fit
  + Buying Intent
  + Timing
  + Economic Relevance
  + Evidence Confidence
  - Negative Signals
```

Example:

```text
Opportunity: Example Corp

Market Fit:             24/25
Problem Fit:            22/25
Solution Fit:           19/20
Buying Intent:          17/20
Timing:                 14/15
Evidence Confidence:     9/10
Negative Signals:       -2

Overall:                103/115
Priority:               HIGH
```

The exact scoring strategy should be configurable and versioned.

---

## 6. Timing Intelligence

A strong organization–offer match does not necessarily imply a current sales opportunity.

MODE therefore treats opportunity as a temporal phenomenon.

```text
90 days ago
  │
  ├── Normal activity
  │
60 days ago
  │
  ├── Relevant hiring begins
  │
30 days ago
  │
  ├── Product expansion announced
  │
14 days ago
  │
  ├── Relevant technical problem appears
  │
Today
  │
  └── HIGH-PROBABILITY OPPORTUNITY WINDOW
```

The system should be able to distinguish:

- **Structural fit** — the organization generally matches the target market.
- **Problem fit** — the organization appears to experience a relevant problem.
- **Intent** — there is evidence of active interest or pressure.
- **Timing** — the relevant conditions have recently emerged or intensified.

---

## 7. Negative Intelligence

MODE explicitly represents evidence against pursuing an organization.

Examples:

```yaml
negative_signals:
  - already_using_competitor
  - insufficient_company_size
  - outside_target_market
  - no_relevant_problem
  - insufficient_economic_capacity
  - stale_evidence
  - incompatible_technology
```

This allows MODE to distinguish:

```text
High ICP fit
+
Low current opportunity
```

from:

```text
High ICP fit
+
Strong problem evidence
+
Recent buying signals
```

Only the latter should necessarily receive high priority.

---

## 8. Evidence Graph

MODE represents observations as relationships between entities.

```text
                    Organization
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
      Technology       Hiring         Funding
          │              │              │
          ▼              ▼              ▼
       Evidence       Evidence       Evidence
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                    Opportunity
```

The graph can eventually incorporate:

- Organizations
- Products
- Technologies
- People
- Jobs
- Events
- Funding
- Regulations
- Problems
- Offers
- Evidence
- Opportunities
- Outcomes

This structure supports provenance, temporal analysis, entity resolution, and cross-source reasoning.

---

## 9. Buyer Discovery

MODE identifies potential buyers **after** establishing the opportunity.

```text
Opportunity
     ↓
Relevant business function
     ↓
Relevant department
     ↓
Relevant seniority
     ↓
Potential decision maker
```

Potential roles include:

- Economic buyer
- Decision maker
- Technical evaluator
- Champion
- Influencer

The appropriate role depends on the opportunity rather than a fixed global title list.

---

## 10. Recommended Actions

MODE should ultimately produce an actionable recommendation rather than only a contact record.

Example:

```text
Priority: HIGH

Why:
The organization recently expanded its engineering
team and publicly identified a software-security problem.

Potential buyer:
VP Engineering

Recommended action:
Request a technical discovery conversation.

Supporting evidence:
1. Engineering expansion
2. Relevant security hiring
3. Public technical problem
4. Compatible technology environment
```

Recommendations should remain traceable to the evidence that produced them.

---

## 11. Configuration

MODE is designed around declarative configuration.

Example:

```yaml
project:
  name: security-opportunity-discovery

offer:
  name: "Automated Software Security Assessment"

market:
  industries:
    - software
    - infrastructure

  geographies:
    - Europe

signals:
  positive:
    - security_hiring
    - software_growth
    - compliance_pressure
    - public_security_problem

  negative:
    - incompatible_stack
    - insufficient_scale

scoring:
  minimum_opportunity_score: 70

output:
  include_evidence: true
  include_recommendation: true
```

A market definition should be portable across discovery runs and version-controlled with the project.

---

## 12. Repository Architecture

A proposed production-oriented layout:

```text
mode/
├── pyproject.toml
├── README.md
├── LICENSE
│
├── src/
│   └── mode/
│       ├── api/
│       ├── cli/
│       │
│       ├── offer/
│       │   ├── models.py
│       │   └── compiler.py
│       │
│       ├── market/
│       │   ├── models.py
│       │   ├── hypotheses.py
│       │   └── segmentation.py
│       │
│       ├── discovery/
│       │   ├── base.py
│       │   └── providers/
│       │
│       ├── evidence/
│       │   ├── models.py
│       │   ├── graph.py
│       │   └── provenance.py
│       │
│       ├── enrichment/
│       │   ├── company.py
│       │   ├── technology.py
│       │   └── people.py
│       │
│       ├── opportunity/
│       │   ├── models.py
│       │   ├── evaluator.py
│       │   └── timing.py
│       │
│       ├── scoring/
│       │   ├── models.py
│       │   └── scorer.py
│       │
│       ├── decision/
│       │   ├── buyer.py
│       │   └── recommendation.py
│       │
│       ├── learning/
│       │   ├── feedback.py
│       │   └── calibration.py
│       │
│       ├── storage/
│       │   └── models.py
│       │
│       └── exporters/
│           ├── json.py
│           ├── csv.py
│           └── api.py
│
├── configs/
├── docs/
├── examples/
└── tests/
```

The implementation may evolve while preserving the separation of concerns represented by these components.

---

## 13. Provider Abstraction

External data sources should be isolated behind provider interfaces.

Potential interfaces include:

```text
DiscoveryProvider
EvidenceProvider
EnrichmentProvider
CompanyProvider
TechnologyProvider
PeopleProvider
NewsProvider
JobProvider
```

A provider should normalize external records into MODE's internal representations.

This permits multiple authorized data sources to be used without coupling the core intelligence engine to a single vendor.

---

## 14. CLI

Example commands:

```bash
mode --help
```

Validate configuration:

```bash
mode config validate configs/example.yaml
```

Compile an offer:

```bash
mode offer compile configs/example.yaml
```

Discover opportunities:

```bash
mode discover configs/example.yaml
```

Rank opportunities:

```bash
mode rank results/
```

Export results:

```bash
mode export results/ --format json
```

The command surface is expected to evolve before a stable release.

---

## 15. API

A future API may expose resources such as:

```text
POST /offers
POST /markets
POST /discovery/jobs
GET  /discovery/jobs/{id}
GET  /opportunities
GET  /opportunities/{id}
GET  /evidence/{id}
POST /outcomes
```

This would allow MODE to serve as an intelligence layer for:

- Web applications
- Mobile applications
- Internal sales systems
- CRMs
- Automated workflows
- Other software agents

---

## 16. Closed-Loop Learning

MODE is designed to learn from downstream commercial outcomes.

```text
Opportunity
     ↓
Sales Action
     ↓
Response
     ↓
Qualification
     ↓
Meeting
     ↓
Proposal
     ↓
Won / Lost
     ↓
Feedback
     ↓
Signal Calibration
     ↓
Improved Opportunity Model
```

Useful feedback signals include:

- Reply
- No reply
- Qualified
- Disqualified
- Meeting booked
- Proposal requested
- Deal won
- Deal lost
- Loss reason

The system can then evaluate which signals are actually predictive of useful commercial outcomes.

---

## 17. Evaluation

Raw lead volume is not the primary success metric.

MODE should be evaluated using metrics such as:

### Opportunity precision

```text
High-quality opportunities
───────────────────────────
Predicted opportunities
```

### Qualification rate

```text
Qualified opportunities
───────────────────────
Discovered organizations
```

### Opportunity conversion

```text
Sales opportunities
────────────────────
Prioritized opportunities
```

### Revenue efficiency

```text
Revenue
────────────────
Discovery cost
```

### Temporal accuracy

How accurately does MODE identify organizations entering relevant opportunity windows?

### Evidence quality

How consistently can high-priority opportunities be supported by independent, recent, and relevant observations?

---

## 18. Testing Strategy

MODE should maintain multiple layers of automated testing.

### Unit Tests

Test:

- Configuration parsing
- Offer modeling
- Signal evaluation
- Scoring
- Deduplication
- Evidence handling
- Opportunity classification
- Recommendation generation

### Integration Tests

Test:

- Discovery providers
- Evidence pipelines
- Entity resolution
- Storage
- End-to-end discovery workflows

### Regression Tests

Production failures should become reproducible regression tests whenever practical.

### Data-Quality Tests

Tests should verify:

- Evidence provenance
- Timestamp integrity
- Duplicate detection
- Missing-data behavior
- Confidence propagation
- Unsupported-inference rejection

---

## 19. Privacy and Responsible Use

MODE is intended for legitimate market research and business development.

Users are responsible for complying with applicable:

- Privacy legislation
- Data-protection requirements
- Anti-spam requirements
- Data-provider agreements
- API terms
- Website terms
- Dataset licenses
- Industry-specific regulations

MODE should favor:

- Authorized APIs
- Licensed datasets
- Public business information
- Minimal personal-data collection
- Data provenance
- Appropriate retention
- Respectful request rates

The system must not be designed to bypass:

- Authentication
- Access controls
- CAPTCHAs
- Paywalls
- Technical restrictions

---

## 20. Security

Security requirements include:

- Never commit credentials to source control.
- Use environment-based secret management.
- Validate external input.
- Audit dependencies.
- Avoid sensitive information in logs.
- Apply appropriate rate limits.
- Minimize stored personal information.
- Preserve auditability.
- Secure API endpoints.
- Pin production dependencies where appropriate.

Example:

```bash
export MODE_API_KEY="..."
```

Credentials should never be embedded in committed configuration files.

---

## 21. Observability

Production deployments should monitor:

- Discovery jobs
- Provider availability
- Evidence collection
- Opportunity counts
- Score distributions
- Rejection rates
- Duplicate rates
- Processing latency
- Data-quality failures
- API usage
- Discovery cost

The objective is to make the complete intelligence pipeline measurable.

---

## 22. Requirements

The initial implementation targets:

- Python 3.10+
- Git

Recommended development tooling includes:

- `pytest`
- `ruff`
- `mypy`
- `bandit`

Additional runtime dependencies should remain minimal and justified by project requirements.

---

## 23. Installation

Clone the repository:

```bash
git clone https://github.com/<OWNER>/<REPOSITORY>.git
cd <REPOSITORY>
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Linux/macOS:

```bash
source .venv/bin/activate
```

Activate it on Windows:

```powershell
.venv\Scripts\activate
```

Install the package:

```bash
pip install -e .
```

Install development dependencies:

```bash
pip install -e ".[dev]"
```

---

## 24. Development

Run the test suite:

```bash
pytest
```

Run linting:

```bash
ruff check .
```

Check formatting:

```bash
ruff format --check .
```

Run type checking:

```bash
mypy .
```

Run security analysis:

```bash
bandit -r src
```

All pull requests should pass the project's automated quality checks.

---

## 25. Roadmap

### Phase 1 — Intelligence Core

- [ ] Offer model
- [ ] Market model
- [ ] Signal model
- [ ] Evidence model
- [ ] Opportunity model
- [ ] Deterministic scoring
- [ ] Configuration validation
- [ ] CLI
- [ ] JSON/CSV output
- [ ] Unit tests
- [ ] CI

### Phase 2 — Discovery

- [ ] Provider abstraction
- [ ] Multiple discovery sources
- [ ] Entity resolution
- [ ] Deduplication
- [ ] Evidence provenance
- [ ] Temporal signals

### Phase 3 — Opportunity Intelligence

- [ ] Opportunity windows
- [ ] Negative intelligence
- [ ] Buyer identification
- [ ] Action recommendations
- [ ] Evidence graph
- [ ] Opportunity dashboard

### Phase 4 — Closed-Loop Learning

- [ ] Outcome tracking
- [ ] Signal calibration
- [ ] Score calibration
- [ ] Conversion modeling
- [ ] Market-model refinement

### Phase 5 — Platform

- [ ] REST API
- [ ] Web interface
- [ ] Authentication
- [ ] Multi-user workspaces
- [ ] Scheduled discovery
- [ ] Integrations
- [ ] Mobile client

---

## 26. Project Status

**Status: MVP / Active Development**

MODE is an evolving research and engineering project.

Configuration schemas, APIs, scoring models, provider interfaces, and storage mechanisms may change before the first stable release.

Production deployments should pin application and dependency versions.

---

## 27. Contributing

Contributions are welcome.

Before submitting a pull request:

1. Keep changes focused.
2. Add or update tests.
3. Preserve evidence provenance.
4. Do not introduce private or sensitive data.
5. Run the test suite.
6. Run linting and formatting checks.
7. Document externally visible behavior.
8. Follow project security and data-use requirements.

Example:

```bash
git checkout -b feature/opportunity-scorer

pytest
ruff check .
ruff format --check .

git commit -m "feat: add opportunity scoring"
git push origin feature/opportunity-scorer
```

Open a pull request after local validation succeeds.

---

## 28. License

MODE is licensed under the **Apache License 2.0**.

See [`LICENSE`](LICENSE) for the complete license text.

---

## 29. Disclaimer

MODE provides software for market research, opportunity discovery, and lead qualification.

An opportunity score is an analytical assessment, not a guarantee that an organization intends to purchase a product or service.

Users are responsible for validating opportunities and complying with applicable laws, regulations, contractual obligations, data licenses, and third-party service policies.

---

## 30. Vision

Traditional sales intelligence asks:

> **Who should I contact?**

MODE asks the preceding question:

> **Where is a commercially relevant problem emerging?**

The long-term system is:

```text
Problem
   ↓
Market
   ↓
Organization
   ↓
Evidence
   ↓
Opportunity
   ↓
Buyer
   ↓
Action
   ↓
Outcome
   ↓
Learning
```

The intended result is a general-purpose **market opportunity discovery layer** that can operate upstream of conventional sales databases, enrichment platforms, CRMs, and outreach systems.

> **MODE is not designed to find more leads.**
>
> **MODE is designed to find better opportunities.**
