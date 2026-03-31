---
name: nexus-talent-assessor
description: "HR talent assessment agent that performs competency-based evaluations, skill gap analysis, 360-feedback synthesis, career path modeling, and individual development plan generation. Produces structured, bias-aware reports ready for HR systems integration."
license: proprietary
compatibility: "Any LLM agent with filesystem access. Accepts CSV, JSON, or plain text input (assessment responses, feedback surveys, job descriptions, competency frameworks). Outputs JSON or Markdown reports. No HRIS integration required."
metadata:
  department: hr
  agents: ["talent-assessor", "ai-coach"]
  price_per_execution: "$2.00"
  version: "2.0.0"
  publishable: true
  categories: ["human-resources", "talent-management", "assessment", "career-development", "skill-gap"]
  trigger_keywords: ["assess talent", "skill gap analysis", "competency evaluation", "360 feedback", "career path", "development plan", "performance review", "hiring assessment", "talent review", "workforce planning"]
allowed-tools: web-search web-fetch filesystem
---

# Nexus Talent Assessor

Data-driven talent assessment that transforms competency data, feedback surveys, and job descriptions into actionable development plans, skill gap reports, and career path recommendations -- with built-in bias detection.

## When to Use This Skill

- **Skill gap analysis**: Compare current workforce competencies against target role requirements
- **360-feedback synthesis**: Aggregate multi-rater feedback into coherent development insights
- **Career path modeling**: Map possible progression routes based on current skills and aspirations
- **Hiring assessment design**: Create competency-based interview frameworks for specific roles
- **Individual development plans (IDPs)**: Generate personalized learning and growth roadmaps
- **Team composition analysis**: Identify capability gaps and redundancies in team structures
- **Succession planning**: Evaluate readiness of candidates for leadership roles

## Instructions

### Step 1: Define Assessment Context

Determine the assessment type and gather inputs:

| Assessment Type | Required Input | Output |
|----------------|----------------|--------|
| Skill Gap Analysis | Current skills + target role description | Gap report with priority rankings |
| 360-Feedback Synthesis | Multi-rater responses (self, manager, peers, reports) | Consolidated report with themes |
| Career Path Modeling | Current role + skills + aspirations + org chart | 2-3 recommended paths with timelines |
| Hiring Framework | Job description + competency model | Structured interview guide + scoring rubric |
| IDP Generation | Assessment results + career goals + available resources | 6-12 month development plan |
| Team Analysis | Team member profiles + team objectives | Capability map + gap/redundancy report |

### Step 2: Execute Assessment

**For Skill Gap Analysis:**

1. Parse the target role requirements into a competency matrix:
   ```json
   {
     "role": "Senior Data Engineer",
     "competencies": [
       {"name": "SQL & Data Modeling", "required_level": 4, "weight": 0.20},
       {"name": "Python/Spark", "required_level": 4, "weight": 0.20},
       {"name": "Cloud Platforms (AWS/GCP)", "required_level": 3, "weight": 0.15},
       {"name": "Data Pipeline Design", "required_level": 4, "weight": 0.20},
       {"name": "Stakeholder Communication", "required_level": 3, "weight": 0.10},
       {"name": "Mentoring & Leadership", "required_level": 2, "weight": 0.15}
     ]
   }
   ```
   Level scale: 1 (Awareness), 2 (Working knowledge), 3 (Proficient), 4 (Expert), 5 (Authority)

2. Map the individual's current competencies to the same framework

3. Calculate gaps:
   - **Critical gaps**: Required level minus current level >= 2, with weight >= 0.15
   - **Development gaps**: Required level minus current level = 1
   - **Strengths**: Current level >= required level
   - **Over-qualification**: Current level exceeds required level by 2+

4. Prioritize gaps by: (gap size x weight x business impact)

**For 360-Feedback Synthesis:**

1. Normalize ratings across rater groups (self-ratings typically inflate by 0.5-1.0 points)
2. Identify convergent themes (mentioned by 2+ rater groups)
3. Flag blind spots (self-rating significantly higher than others)
4. Flag hidden strengths (others rate significantly higher than self)
5. Extract verbatim quotes that illustrate key themes (anonymized)
6. Generate development priorities based on themes, not isolated comments

**For Career Path Modeling:**

1. Assess current competency profile (technical + behavioral + leadership)
2. Map to possible next roles using competency overlap analysis:
   - **Adjacent roles** (70%+ skill overlap, 6-12 month transition)
   - **Stretch roles** (50-70% overlap, 12-24 month development needed)
   - **Pivot roles** (30-50% overlap, significant reskilling required)
3. For each path, identify:
   - Skills to develop
   - Experiences to gain (projects, rotations, mentoring)
   - Certifications or training recommended
   - Estimated timeline
   - Market demand for target role

### Step 3: Apply Bias Detection

Flag potential bias indicators in the assessment:
- **Recency bias**: Feedback heavily weighted toward recent events
- **Halo/horn effect**: All ratings clustered at one extreme
- **Similarity bias**: Higher ratings for similar backgrounds
- **Gender-coded language**: "Aggressive" vs "assertive", "emotional" vs "passionate"
- **Central tendency**: All ratings clustered around middle with no differentiation

### Step 4: Generate Structured Output

```json
{
  "assessment_type": "skill_gap_analysis",
  "subject": "Jane Doe - Data Analyst seeking Senior Data Engineer role",
  "date": "2026-03-31",
  "overall_readiness": "67% -- Development needed in 2 critical areas",
  "competency_map": [
    {
      "competency": "SQL & Data Modeling",
      "required": 4,
      "current": 3,
      "gap": 1,
      "priority": "MEDIUM",
      "development_action": "Complete advanced SQL course + lead one data model redesign project"
    },
    {
      "competency": "Python/Spark",
      "required": 4,
      "current": 2,
      "gap": 2,
      "priority": "CRITICAL",
      "development_action": "6-month structured learning path: Python intermediate -> PySpark -> real project. Assign mentor from Data Engineering team."
    }
  ],
  "development_plan": {
    "timeline": "12 months",
    "phase_1": {"months": "1-3", "focus": "Python proficiency", "actions": ["Complete DataCamp Python track", "Pair programming with senior engineer 2x/week"]},
    "phase_2": {"months": "4-6", "focus": "Spark + pipeline design", "actions": ["PySpark project assignment", "Shadow existing pipeline maintenance"]},
    "phase_3": {"months": "7-12", "focus": "Independent delivery + leadership", "actions": ["Lead pipeline migration project", "Mentor one junior analyst"]}
  },
  "bias_check": {
    "flags": [],
    "note": "Assessment based on objective competency mapping -- no rater-dependent bias vectors identified"
  }
}
```

## Example Input/Output

**Input**: "We have 360-feedback data for a product manager. Self-rated 4.5/5 overall. Manager rated 3.5. Peers average 3.8. Direct reports average 3.2. Help us understand what's going on and create a development plan."

**Output**: Report identifying:
- **Blind spot detected**: Self-rating 0.7-1.3 points above all other rater groups
- **Key theme from reports**: "Does not delegate effectively -- takes over tasks when deadlines approach" (3 of 4 reports mention this)
- **Key theme from peers**: "Strong strategic thinker but sometimes bypasses process" (2 of 3 peers)
- **Manager alignment**: "Needs to develop team more, excellent individual contributor"
- **Development plan**: Focus on delegation (coaching-style leadership training), process adherence (pair with operations PM), and self-awareness (monthly feedback check-ins)

## Edge Cases

- **Insufficient data**: If fewer than 3 raters in any 360 group, flag that group's data as unreliable
- **All ratings identical**: Flag as potential survey fatigue or social desirability bias
- **Role does not exist yet**: Use closest comparable role + industry benchmarks for competency framework
- **Non-standard competency frameworks**: Map to universal framework (technical, behavioral, leadership) then back to client's taxonomy
- **Confidentiality concerns**: Never include rater-identifiable information in output. Aggregate all feedback before reporting.
- **Cross-cultural teams**: Note that feedback norms vary by culture (e.g., direct vs indirect feedback styles) and adjust interpretation

## What Sets This Apart

- **Bias detection built in** -- flags language patterns and rating anomalies that indicate bias
- **Prioritized gaps** with weighted scoring, not just a list of deficiencies
- Generates **complete development plans** with phased timelines and specific actions
- **360-feedback synthesis** goes beyond averaging -- identifies themes, blind spots, and hidden strengths
- **Career path modeling** includes market demand data and realistic timelines
- All outputs are **HRIS-friendly** JSON, ready for system integration

## Pricing

- Per-execution: $2.00 (single assessment or analysis)
- Team assessment (5-15 members): $8.00
- Volume: 20% discount at 50+ assessments/month
- Enterprise: Annual license for organization-wide talent reviews

## Output Format

Default: JSON. Supports `format: "markdown"` for manager review, `format: "csv"` for HRIS bulk import.
