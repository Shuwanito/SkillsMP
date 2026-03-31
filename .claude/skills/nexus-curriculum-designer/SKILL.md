---
name: nexus-curriculum-designer
description: "AI curriculum design agent that creates adaptive learning paths, competency-based assessments, and standards-aligned educational content. Supports Bloom's taxonomy mapping, Universal Design for Learning (UDL), scaffolded progressions, and multiple educational frameworks (LOMLOE, Common Core, IB, Cambridge)."
license: proprietary
compatibility: "Any LLM agent with web access. Outputs JSON, Markdown, or SCORM-compatible XML. No runtime dependencies -- generates content documents, not executable code."
metadata:
  department: education
  agents: ["edu-curriculum", "ped-learning"]
  price_per_execution: "$1.00"
  version: "2.0.0"
  publishable: true
  categories: ["education", "curriculum-design", "learning-paths", "assessment", "instructional-design"]
  trigger_keywords: ["design curriculum", "create learning path", "lesson plan", "course outline", "assessment design", "learning objectives", "Bloom's taxonomy", "competency map", "educational content", "adaptive learning"]
allowed-tools: web-search web-fetch filesystem
---

# Nexus Curriculum Designer

Professional curriculum design that produces structured, standards-aligned learning paths with built-in differentiation, assessment rubrics, and adaptive scaffolding -- ready for LMS import or direct use.

## When to Use This Skill

- **Course creation**: Design a full course from topic description and target audience
- **Learning path design**: Create multi-week adaptive progressions with prerequisites
- **Assessment generation**: Build rubrics, quizzes, and project-based evaluations
- **Standards alignment**: Map existing content to educational frameworks (Bloom's, LOMLOE, Common Core)
- **Accessibility adaptation**: Redesign materials for UDL compliance and diverse learners
- **Corporate training modules**: Structure onboarding or upskilling programs

## Instructions

### Step 1: Gather Requirements

Collect from the user or infer from context:
- **Subject area**: e.g., "Python programming", "7th grade biology", "compliance training"
- **Target audience**: Age/level, prior knowledge, professional role
- **Duration**: Total hours, sessions per week, session length
- **Standards framework** (if any): LOMLOE, Common Core, IB, Cambridge, ISTE, custom competency model
- **Delivery format**: Self-paced online, instructor-led, blended, microlearning
- **Assessment preference**: Formative, summative, portfolio, project-based, exam

### Step 2: Design Learning Objectives

For each module/unit, write objectives using Bloom's Taxonomy verbs:

| Level | Verbs | Example |
|-------|-------|---------|
| Remember | List, define, identify | "List the 5 principles of OOP" |
| Understand | Explain, summarize, compare | "Explain why encapsulation improves maintainability" |
| Apply | Implement, use, demonstrate | "Implement a class hierarchy for a library system" |
| Analyze | Differentiate, examine, test | "Analyze trade-offs between inheritance and composition" |
| Evaluate | Judge, critique, assess | "Evaluate whether a given design follows SOLID principles" |
| Create | Design, build, develop | "Design a full application architecture using design patterns" |

Ensure the course progresses from lower to higher Bloom's levels across modules.

### Step 3: Structure the Curriculum

Generate this structure:

```json
{
  "course": {
    "title": "Introduction to Object-Oriented Programming",
    "audience": "University CS students, year 1",
    "duration": "12 weeks, 3 hours/week",
    "prerequisites": ["Basic Python syntax", "Variables and control flow"],
    "standards_alignment": ["ACM CS2013 PL/OOP", "Bloom's L1-L6"],
    "modules": [
      {
        "id": "M01",
        "title": "Classes and Objects",
        "week": 1,
        "duration_hours": 3,
        "objectives": [
          {"bloom_level": "Remember", "text": "Define class, object, instance, and attribute"},
          {"bloom_level": "Apply", "text": "Create a Python class with __init__ and methods"}
        ],
        "topics": ["Class syntax", "Constructors", "Instance vs class attributes", "Methods"],
        "activities": [
          {"type": "guided_practice", "description": "Build a Student class step by step", "duration_min": 30},
          {"type": "independent_practice", "description": "Create a BankAccount class with deposit/withdraw", "duration_min": 45}
        ],
        "assessment": {
          "type": "formative",
          "description": "Submit BankAccount class -- auto-graded against 5 test cases",
          "rubric": [
            {"criterion": "Correct __init__", "weight": 20},
            {"criterion": "Methods work correctly", "weight": 40},
            {"criterion": "Edge case handling", "weight": 25},
            {"criterion": "Code style (PEP8)", "weight": 15}
          ]
        },
        "differentiation": {
          "struggling": "Provide class template with TODO comments",
          "advanced": "Add transfer method between two accounts with validation"
        }
      }
    ],
    "final_assessment": {
      "type": "project",
      "description": "Design and implement a library management system using OOP principles",
      "rubric_summary": "Architecture (30%), Functionality (30%), Code quality (20%), Documentation (20%)"
    }
  }
}
```

### Step 4: Apply UDL Principles

For each module, include:
- **Multiple means of engagement**: Choice in topics/projects, real-world relevance connections
- **Multiple means of representation**: Text + diagrams + video links + code examples
- **Multiple means of action/expression**: Written code, oral explanation, diagram, peer teaching

### Step 5: Generate Supplementary Materials

- **Rubrics**: Detailed scoring criteria for each assessment
- **Prerequisite diagnostic**: Quick quiz to verify readiness for the course
- **Pacing guide**: Week-by-week schedule with buffer weeks for review
- **Resource list**: Recommended readings, tools, and practice platforms

## Example Input/Output

**Input**: "Design a 6-week corporate onboarding curriculum for new data analysts. They know Excel but not SQL or Python. 2 hours per day, 5 days a week."

**Output**: Structured curriculum with:
- Week 1: SQL fundamentals (SELECT, JOIN, GROUP BY) with company database examples
- Week 2: Advanced SQL (subqueries, window functions, CTEs) with real reporting tasks
- Week 3: Python basics (pandas, data types, file I/O) bridging from Excel concepts
- Week 4: Data visualization (matplotlib, plotly) reproducing existing company dashboards
- Week 5: Integration project -- build an automated report pipeline
- Week 6: Presentation + peer review + assessment
- Each day includes: 30min instruction, 60min hands-on, 30min review
- Rubrics, diagnostic quiz, and resource list included

## Edge Cases

- **No standards framework specified**: Default to Bloom's taxonomy alignment only
- **Mixed-level audience**: Design with core track + extension activities, include diagnostic pre-test
- **Very short duration (<4 hours total)**: Switch to microlearning format with focused skill targets
- **Subject outside common domains**: Use generic instructional design principles, flag that domain-specific validation is recommended
- **Multiple languages/cultures**: Note localization needs, avoid culture-specific examples in base design
- **Compliance/regulatory training**: Include mandatory completion tracking and pass/fail thresholds

## What Sets This Apart

- Produces **complete, structured curricula** -- not just topic lists or outlines
- Every objective is mapped to a **Bloom's taxonomy level** with appropriate assessment
- Built-in **differentiation** for struggling and advanced learners in every module
- Generates **rubrics with weighted criteria** ready for LMS upload
- Supports **multiple standards frameworks** (LOMLOE, Common Core, IB, corporate competency models)
- Includes **UDL compliance** as a standard feature, not an add-on

## Pricing

- Per-execution: $1.00 (single module or short course design)
- Full course design (12+ modules): $5.00
- Volume: 20% discount at 50+ designs/month
- Enterprise: Custom rate for institution-wide curriculum overhauls

## Output Format

Default: JSON. Supports `format: "markdown"` for human review, `format: "scorm"` for LMS-compatible XML package.
