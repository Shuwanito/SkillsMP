---
name: nexus-clinical-case-gen
description: "Medical clinical case generator for healthcare education. Creates realistic, evidence-based patient scenarios with progressive disclosure, differential diagnosis trees, OSCE-format evaluations, and PubMed-referenced learning points. Covers internal medicine, emergency, pediatrics, surgery, and psychiatry."
license: proprietary
compatibility: "Any LLM agent with web access. Outputs JSON or Markdown. No medical database subscription required -- references PubMed and open clinical guidelines. Content suitable for medical students, residents, and CME programs."
metadata:
  department: healthcare
  agents: ["medical-content", "clinical-simulator"]
  price_per_execution: "$5.00"
  version: "2.0.0"
  publishable: true
  categories: ["healthcare", "medical-education", "clinical-simulation", "OSCE", "CME"]
  trigger_keywords: ["clinical case", "patient scenario", "medical simulation", "OSCE station", "differential diagnosis", "clinical vignette", "medical education", "CME case", "patient presentation", "clinical reasoning"]
allowed-tools: web-search web-fetch filesystem
---

# Nexus Clinical Case Generator

Creates publication-quality clinical cases for medical education -- complete with patient presentation, progressive history disclosure, lab/imaging results, differential diagnosis reasoning, and evidence-based learning points with citations.

## When to Use This Skill

- **Medical school teaching**: Generate cases for problem-based learning (PBL) sessions
- **OSCE station design**: Create structured clinical examination scenarios with marking schemes
- **Residency training**: Build complex multi-system cases for morning report or case conferences
- **CME content**: Develop Continuing Medical Education cases with self-assessment questions
- **Board exam prep**: Generate USMLE/PLAB/MIR-style clinical vignettes with answer explanations
- **Nursing/allied health education**: Adapt complexity for different healthcare disciplines

## Instructions

### Step 1: Define Case Parameters

Gather from the user:
- **Specialty**: Internal medicine, emergency, pediatrics, OB/GYN, surgery, psychiatry, family medicine
- **Target learner level**: Medical student (pre-clinical/clinical), resident (PGY1-5), practicing physician
- **Chief complaint or topic**: e.g., "chest pain", "Type 2 diabetes management", "pediatric fever"
- **Case complexity**: Straightforward (single diagnosis), moderate (2-3 differentials), complex (multi-system, atypical presentation)
- **Format**: Progressive disclosure, OSCE station, clinical vignette (MCQ), or full case study

### Step 2: Build Patient Presentation

Create a realistic patient with:

```
Demographics: Age, sex, occupation, relevant social context
Chief complaint: In patient's own words ("I've had this chest pain for 3 hours")
History of present illness: Onset, location, duration, character, aggravating/alleviating factors, associated symptoms
Past medical history: Relevant conditions, surgeries, medications
Family history: Relevant genetic/familial conditions
Social history: Smoking, alcohol, occupation, living situation, relevant exposures
Review of systems: Pertinent positives and negatives that guide differential
```

Ensure the presentation is:
- **Realistic**: Patients do not present with textbook descriptions. Include some ambiguity.
- **Inclusive**: Vary demographics across cases. Include diverse presentations.
- **Appropriately complex**: Match the target learner level.

### Step 3: Design Progressive Disclosure Stages

Structure the case in 4-6 stages that simulate clinical reasoning:

| Stage | Content | Learner Task |
|-------|---------|--------------|
| 1. Initial presentation | Chief complaint + brief HPI | Generate initial differential (5-7 diagnoses) |
| 2. Focused history | Complete HPI + PMH + social history | Narrow differential to top 3-4 |
| 3. Physical examination | Vital signs + relevant exam findings | Identify key findings, refine differential |
| 4. Investigations | Lab results + imaging (released sequentially) | Interpret results, reach working diagnosis |
| 5. Management | Treatment options + response | Select and justify management plan |
| 6. Follow-up | Outcome + complications (if any) | Reflect on clinical reasoning, identify learning points |

At each stage, provide:
- The new information revealed
- Expected learner actions
- Teaching points for that stage
- Common mistakes learners make at this point

### Step 4: Build Differential Diagnosis Tree

```json
{
  "differential_diagnosis": {
    "must_not_miss": [
      {"diagnosis": "Acute MI", "key_features": ["Chest pain", "Diaphoresis", "ECG changes"], "probability": "35%"}
    ],
    "most_likely": [
      {"diagnosis": "Unstable angina", "key_features": ["Exertional pain at rest", "Cardiac risk factors"], "probability": "30%"}
    ],
    "consider": [
      {"diagnosis": "Pulmonary embolism", "key_features": ["Sudden onset", "Tachycardia", "Recent immobility"], "probability": "15%"},
      {"diagnosis": "Aortic dissection", "key_features": ["Tearing pain", "BP differential"], "probability": "5%"}
    ],
    "less_likely_but_important": [
      {"diagnosis": "Tension pneumothorax", "key_features": ["Absent breath sounds", "Hypotension"], "probability": "3%"}
    ]
  }
}
```

### Step 5: Generate Assessment Components

**For OSCE format, include:**
- Standardized patient script (what the SP says in response to specific questions)
- Examiner marking scheme with weighted criteria
- Expected time allocation (typically 8-12 minutes per station)
- Global rating scale descriptors

**For MCQ/vignette format, include:**
- Stem (case presentation in 100-200 words)
- 5 answer options (1 best answer, 1 close distractor, 3 reasonable distractors)
- Explanation for correct answer with evidence citation
- Explanation for why each distractor is incorrect

**For PBL/case study format, include:**
- Discussion questions for each stage
- Key learning objectives mapped to curriculum
- Suggested facilitator guide with prompts
- Recommended reading with PubMed references (PMIDs)

### Step 6: Add Evidence Base

For each clinical decision point, reference:
- Current clinical guidelines (e.g., ESC, AHA, NICE, WHO)
- Key studies by name where applicable (e.g., "RECOVERY trial for dexamethasone in COVID")
- PubMed IDs for primary evidence when available
- Note the date of guidelines referenced

## Example Input/Output

**Input**: "Generate an OSCE station for medical students on acute appendicitis in a 22-year-old. 10 minutes, moderate complexity."

**Output** (abbreviated):
- **Patient**: 22M presenting with 12-hour history of periumbilical pain migrating to RLQ, low-grade fever, nausea
- **SP script**: Responds to history questions with appropriate details, guards RLQ on exam
- **Expected actions**: Focused abdominal exam, assess for peritoneal signs, order CBC + CRP + CT/US
- **Marking scheme**: History taking (25%), Examination technique (25%), Differential diagnosis (20%), Investigation plan (15%), Communication (15%)
- **Key differential**: Appendicitis (most likely), mesenteric lymphadenitis, Meckel diverticulitis, right ovarian pathology (if female)
- **Teaching points**: Alvarado score, role of imaging, criteria for surgical vs conservative management
- **References**: NICE NG164, PMID 32386550 (CODA trial -- antibiotics vs surgery)

## Edge Cases

- **Rare diseases**: Flag rarity, include prevalence data, note that case is designed for teaching awareness not probability-based reasoning
- **Pediatric cases**: Adjust vital signs to age-appropriate normals, use age-appropriate communication in SP script
- **Psychiatric presentations**: Include mental state examination instead of physical exam, add risk assessment component
- **Conditions with no definitive test**: Emphasize clinical diagnosis criteria (e.g., migraine, fibromyalgia) and importance of ruling out red flags
- **Controversial management**: Present both approaches with evidence for each, do not endorse one over the other
- **Culturally sensitive topics**: Handle with appropriate sensitivity, include cultural competency teaching points

## Quality Standards

- All clinical information must be medically accurate and consistent within the case
- Vital signs, lab values, and imaging findings must be internally consistent with the diagnosis
- Distractors must be clinically plausible, not obviously wrong
- Drug dosages and treatment protocols must match current guidelines
- Cases must not reinforce stereotypes (e.g., specific diagnoses exclusively in certain demographics)

## What Sets This Apart

- **Progressive disclosure** simulates real clinical reasoning, not just static vignettes
- **Differential diagnosis trees** with probability estimates teach Bayesian reasoning
- **OSCE-ready** with complete SP scripts and marking schemes -- deploy directly
- **Evidence-based** with PubMed references, not just clinical intuition
- **Multi-format**: Same clinical scenario can be output as OSCE, MCQ, PBL, or full case study
- **Quality-controlled**: Internal consistency checks on vitals, labs, and clinical timeline

## Pricing

- Per-execution: $5.00 (single case, any format)
- Case set (5 cases, themed): $20.00
- CME module (10 cases + assessment + certificate framework): $40.00
- Volume: 20% discount at 20+ cases/month
- Enterprise: Institutional license for unlimited generation

## Output Format

Default: JSON with all stages, differentials, and assessment components. Supports `format: "markdown"` for faculty review, `format: "moodle-xml"` for LMS import of MCQ components.
