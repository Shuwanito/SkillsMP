---
name: nexus-legal-analyzer
description: "Legal analysis agent with deep knowledge of EU, Spanish, and international regulatory frameworks. Performs contract clause analysis, GDPR/EU AI Act compliance checks, regulatory risk assessment, and legislative change monitoring. Returns structured findings with risk ratings and remediation guidance."
license: proprietary
compatibility: "Any LLM agent with web and filesystem access. Analyzes text documents (PDF, DOCX, TXT, Markdown). Outputs JSON or Markdown reports. No external legal database subscription required -- uses web search for current regulations."
metadata:
  department: legal
  agents: ["legal-rag", "legal-compliance"]
  price_per_execution: "$3.00"
  version: "2.0.0"
  publishable: true
  categories: ["legal", "compliance", "contracts", "regulation", "GDPR", "AI-governance"]
  trigger_keywords: ["analyze contract", "legal review", "compliance check", "GDPR audit", "EU AI Act", "regulatory risk", "contract clauses", "terms of service review", "privacy policy", "legal compliance", "data protection"]
allowed-tools: web-search web-fetch filesystem
---

# Nexus Legal Analyzer

Structured legal analysis for contracts, compliance, and regulatory risk -- delivering clause-by-clause review, risk scores, and actionable remediation steps, not vague legal opinions.

## When to Use This Skill

- **Contract review**: Analyze vendor contracts, SaaS agreements, NDAs, or employment contracts
- **GDPR compliance check**: Audit data processing activities, privacy policies, or consent mechanisms
- **EU AI Act assessment**: Classify AI systems by risk tier and identify compliance gaps
- **Terms of service review**: Flag problematic clauses in ToS/EULA before signing
- **Regulatory change monitoring**: Check if recent legislation affects current operations
- **Data processing agreement (DPA) audit**: Verify DPAs meet GDPR Article 28 requirements

## Instructions

### Step 1: Identify Analysis Type

Determine which analysis mode to use:

| Mode | Input | Output |
|------|-------|--------|
| Contract Review | Contract text or file path | Clause-by-clause analysis with risk flags |
| Compliance Check | Description of system/process + regulation | Gap analysis with remediation steps |
| Regulatory Risk | Business description + jurisdiction | Risk matrix with applicable regulations |
| Legislative Monitor | Topic + jurisdiction + date range | Summary of relevant changes and impacts |

### Step 2: Execute Analysis

**For Contract Review:**

1. Extract all clauses and categorize them:
   - Liability and indemnification
   - Data protection and privacy
   - Intellectual property rights
   - Termination and exit conditions
   - Service level agreements (SLAs)
   - Payment terms and penalties
   - Non-compete and non-solicitation
   - Governing law and dispute resolution

2. For each clause, evaluate:
   - **Fairness**: Is it balanced or heavily favoring one party?
   - **Completeness**: Are expected protections missing?
   - **Risk level**: What is the worst-case exposure?
   - **Market standard**: Is this typical or unusual for this contract type?
   - **Red flags**: Auto-renewal traps, unlimited liability, broad IP assignment, weak termination rights

3. Identify **missing clauses** that should be present (e.g., force majeure, data breach notification, audit rights).

**For GDPR Compliance Check:**

Evaluate against these requirements:
- Lawful basis for processing (Article 6) -- is it identified and appropriate?
- Data minimization (Article 5) -- is only necessary data collected?
- Consent mechanism (Article 7) -- is consent freely given, specific, informed, unambiguous?
- Data subject rights (Articles 15-22) -- are access, rectification, erasure, portability supported?
- Data Protection Impact Assessment (Article 35) -- is a DPIA required and completed?
- Cross-border transfer safeguards (Chapter V) -- SCCs, adequacy decisions, or BCRs in place?
- Data breach procedures (Articles 33-34) -- 72-hour notification process documented?
- DPO appointment (Article 37) -- required or voluntarily appointed?

**For EU AI Act Assessment:**

1. Classify the AI system:
   - **Unacceptable risk** (Article 5): Social scoring, real-time biometric identification in public
   - **High risk** (Annex III): HR recruitment, credit scoring, medical devices, law enforcement
   - **Limited risk** (Article 50): Chatbots, deepfakes, emotion recognition
   - **Minimal risk**: Spam filters, game AI, recommendation engines

2. For high-risk systems, check:
   - Risk management system (Article 9)
   - Data governance and quality (Article 10)
   - Technical documentation (Article 11)
   - Record-keeping and logging (Article 12)
   - Transparency and information (Article 13)
   - Human oversight mechanisms (Article 14)
   - Accuracy, robustness, cybersecurity (Article 15)

### Step 3: Generate Structured Report

```json
{
  "analysis_type": "contract_review",
  "document": "SaaS Vendor Agreement - CloudCo",
  "jurisdiction": "EU / Spain",
  "overall_risk": "MEDIUM-HIGH",
  "summary": "Contract is vendor-favorable with weak exit terms and broad IP license. Data protection clauses meet minimum GDPR but lack breach notification timeline.",
  "findings": [
    {
      "id": "LA-001",
      "clause": "Section 8.2 - Limitation of Liability",
      "risk": "HIGH",
      "issue": "Vendor liability capped at 1 month of fees. No carve-outs for data breaches, IP infringement, or gross negligence.",
      "market_standard": "Typical cap is 12 months of fees with carve-outs for data breach and willful misconduct.",
      "recommendation": "Negotiate cap to 12 months minimum. Add carve-outs for data breach (uncapped or 2x annual fees) and willful misconduct.",
      "impact_if_ignored": "In a data breach affecting 10K+ users, maximum recovery would be ~$2,000 regardless of damages."
    },
    {
      "id": "LA-002",
      "clause": "MISSING - Data Breach Notification",
      "risk": "HIGH",
      "issue": "No clause requiring vendor to notify of data breaches within a specific timeframe.",
      "market_standard": "24-48 hours notification to customer, enabling GDPR 72-hour notification to DPA.",
      "recommendation": "Add clause: Vendor must notify Customer of any personal data breach within 24 hours of discovery.",
      "impact_if_ignored": "Customer may fail GDPR Article 33 notification deadline, risking fines up to 2% of global turnover."
    }
  ],
  "missing_clauses": [
    "Data breach notification timeline",
    "Audit rights for data processing activities",
    "Data portability and return upon termination"
  ],
  "negotiation_priorities": [
    "1. Add data breach notification clause (risk: regulatory fine)",
    "2. Increase liability cap with carve-outs (risk: financial exposure)",
    "3. Add data return/deletion upon termination (risk: vendor lock-in)"
  ]
}
```

## Example Input/Output

**Input**: "Check if our AI chatbot for customer support needs to comply with the EU AI Act and what we need to do"

**Output**: Report identifying:
- Classification: Limited risk (Article 50) -- chatbot that interacts with humans
- Required: Transparency obligation -- users must be informed they are interacting with AI
- Required: If chatbot generates synthetic text published as human-written, must be labeled as AI-generated
- Recommended (not required): Maintain technical documentation of training data and model behavior
- Timeline: Obligations effective August 2026 for limited-risk systems
- Action items: Add disclosure banner, update privacy policy, document model card

## Edge Cases

- **Multiple jurisdictions**: Analyze against all applicable frameworks, note conflicts between them
- **Non-English contracts**: Process the text as-is, but flag that legal terminology may carry jurisdiction-specific meaning
- **Incomplete documents**: Analyze what is provided, list sections that appear to be missing or referenced but not included
- **Highly regulated industries** (financial services, healthcare): Flag that sector-specific regulations (MiFID II, MDR) may apply beyond general frameworks
- **Pre-contractual negotiations**: Provide redline suggestions, not just risk analysis
- **Rapidly changing regulations**: Always include the date of analysis and note that findings are based on regulations as of that date

## Important Disclaimers

- This skill provides **legal analysis assistance**, not legal advice
- All outputs should be reviewed by qualified legal counsel before acting
- Regulatory interpretations may vary by jurisdiction and are subject to change
- Confidence scores reflect analytical certainty, not legal certainty

## What Sets This Apart

- **Clause-by-clause** structured analysis, not just general observations
- Compares findings against **market standards** for that contract type
- Identifies **missing clauses** that should be present, not just problems with existing ones
- Provides **specific remediation language** and negotiation priorities
- Covers **EU AI Act** risk classification -- a rapidly evolving area most tools do not address
- Includes **financial impact** estimates for ignoring findings

## Pricing

- Per-execution: $3.00 (single document or compliance check)
- Full contract negotiation support (multiple rounds): $10.00
- Volume: 20% discount at 50+ analyses/month
- Enterprise: Monthly retainer with unlimited analyses

## Output Format

Default: JSON. Supports `format: "markdown"` for human-readable report or `format: "redline"` for contract markup suggestions.
