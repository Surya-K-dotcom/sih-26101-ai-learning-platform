# PROJECT\_CONTEXT.md

## 1\. Project Identity

**Project Name:** AI-Enabled Competency Learning Platform for Official Statistics  
**SIH:** Smart India Hackathon 2026  
**Problem Statement ID:** 26101

**Problem Statement:**

Develop an AI-enabled learning platform that identifies competency gaps, recommends personalized training through integration with the iGOT Karmayogi ecosystem, and generates quizzes and MCQs from uploaded learning materials to strengthen capacity building in India's Official Statistical System.

\---

# 2\. Project Objective

Build a **competency intelligence and personalized capacity-building platform**, not a generic LMS.

The platform must:

1. Build an official's competency profile.
2. Determine competencies required by their role.
3. Assess current competencies.
4. Identify competency/skill gaps.
5. Retrieve relevant iGOT learning resources.
6. Recommend personalized training.
7. Generate personalized learning paths.
8. Process uploaded learning materials.
9. Generate and validate AI-based MCQs/quizzes.
10. Evaluate learning performance.
11. Update competency levels using assessment evidence.
12. Recalculate gaps and recommendations.
13. Provide a competency-aware AI assistant.
14. Support future skill intelligence.
15. Provide secure official and administrator dashboards.

\---

# 3\. Users

## Official

Can:

* Manage/view professional profile.
* Take competency assessments.
* View competency profile.
* View skill gaps.
* Receive course recommendations.
* Follow learning paths.
* Upload learning material where permitted.
* Generate quizzes/MCQs.
* Attempt quizzes.
* View feedback and progress.
* Interact with competency-aware AI assistant.

## Administrator

Can:

* Manage users.
* Manage roles.
* Manage competency framework.
* Manage role-to-competency mappings.
* Manage course metadata/mappings.
* Manage questions.
* Manage assessments.
* View system analytics.
* View audit logs.

Initial roles:

* `OFFICIAL`
* `ADMIN`

Architecture must support future roles without redesign.

\---

# 4\. Core Features

### Competency Lifecycle

```text
PROFILE
→ ASSESS
→ COMPETENCY
→ SKILL GAP
→ iGOT COURSE
→ RECOMMEND
→ LEARN
→ ASSESS
→ UPDATE COMPETENCY
→ RECOMMEND AGAIN
```

### AI Learning Loop

```text
UPLOAD MATERIAL
→ EXTRACT
→ GENERATE MCQs
→ VALIDATE
→ QUIZ
→ ANALYZE PERFORMANCE
→ UPDATE COMPETENCY
→ RECOMMEND AGAIN
```

Core modules:

* Authentication
* RBAC
* Official Profile
* Competency Framework
* Role-to-Competency Mapping
* Competency Assessment
* Competency Engine
* Skill-Gap Engine
* iGOT Integration Layer
* iGOT Course Retrieval
* Course-to-Competency Mapping
* Recommendation Engine
* Personalized Learning Path
* Document Processing
* AI MCQ Generation
* Quiz Engine
* Quiz Evaluation
* Competency Updates
* Competency-Aware AI Assistant
* Future Skill Intelligence
* Official Dashboard
* Admin Dashboard
* Audit Logging

\---

# 5\. Technology Stack

## Frontend

* Next.js
* React
* TypeScript

## Backend

* Python
* FastAPI

## Database

* PostgreSQL
* pgvector

## AI

* Configurable LLM API
* Structured outputs
* Embedding model
* RAG

AI provider must remain configurable.

## ML

* Python
* scikit-learn where genuinely useful

## Document Processing

* PyMuPDF for PDF
* python-docx for DOCX
* python-pptx for PPTX

\---

# 6\. Architecture

```text
Users
  ↓
Next.js / React
  ↓
FastAPI
  ├── Authentication / RBAC
  ├── Official Profile
  ├── Competency Engine
  ├── Assessment Engine
  ├── Skill-Gap Engine
  ├── Recommendation Engine
  ├── Learning Path Engine
  ├── Document Processing
  ├── Quiz Engine
  ├── AI Assistant
  └── iGOT Integration
        ↓
PostgreSQL + pgvector
        +
AI / ML Services
        +
iGOT Adapter
```

The application is competency-centric.

AI is a supporting intelligence layer, not the entire application.

Deterministic business logic must remain outside the LLM.

\---

# 7\. Recommended Folder Structure

The exact implementation structure may evolve, but domain separation must be preserved.

```text
project-root/
│
├── frontend/
│   ├── app/
│   ├── components/
│   ├── features/
│   ├── lib/
│   ├── hooks/
│   └── types/
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── core/
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── services/
│   │   ├── repositories/
│   │   ├── modules/
│   │   │   ├── auth/
│   │   │   ├── officials/
│   │   │   ├── competencies/
│   │   │   ├── assessments/
│   │   │   ├── recommendations/
│   │   │   ├── learning/
│   │   │   ├── documents/
│   │   │   ├── quizzes/
│   │   │   ├── assistant/
│   │   │   └── igot/
│   │   └── main/
│   │
│   └── tests/
│
├── docs/
│
├── scripts/
│
├── .env.example
├── ARCHITECTURE.md
├── PROJECT\_CONTEXT.md
└── README.md
```

The structure may be adjusted when necessary, but domain boundaries must not be casually removed.

\---

# 8\. Database Entities

Core entities:

```text
users
official\_profiles
roles
departments
designations

competency\_domains
competencies
competency\_levels
competency\_skills

role\_competencies
official\_competencies
competency\_evidence

assessments
assessment\_questions
assessment\_attempts
assessment\_answers

skill\_gaps

courses
course\_competencies
course\_metadata
course\_recommendations

learning\_paths
learning\_path\_items
learning\_progress

documents
document\_chunks
document\_embeddings

question\_banks
questions
question\_options

quizzes
quiz\_questions
quiz\_attempts
quiz\_answers
quiz\_feedback

ai\_conversations
ai\_messages

future\_skills

audit\_logs
```

Competency records must preserve evidence/history rather than only storing a current unexplained score.

\---

# 9\. API Conventions

Base path:

```text
/api/v1/
```

Use REST/JSON unless there is a strong architectural reason otherwise.

Conventions:

* Consistent HTTP status codes.
* Request validation at API boundaries.
* Typed request/response schemas.
* Authentication on protected endpoints.
* RBAC on role-sensitive endpoints.
* Resource ownership checks.
* Consistent error response structure.
* No secrets in responses.
* No direct database access from frontend.
* Business logic belongs in backend services.
* External integrations must be behind service/adaptor interfaces.

Representative domains:

```text
/auth/\*
/officials/\*
/competencies/\*
/roles/\*
/assessments/\*
/competency-gaps/\*
/igot/\*
/recommendations/\*
/learning-path/\*
/documents/\*
/question-banks/\*
/quizzes/\*
/assistant/\*
/admin/\*
```

\---

# 10\. AI Architecture

AI pipeline:

```text
Application
   ↓
AI Service
   ↓
Context / Prompt Builder
   ↓
LLM Provider
   ↓
Structured Output
   ↓
Schema Validation
   ↓
Business Validation
   ↓
Application Logic
```

LLMs must not directly mutate critical system state.

AI-generated data must be validated before persistence.

AI should primarily handle:

* Semantic understanding
* Document understanding
* MCQ generation
* Semantic matching
* RAG
* AI assistant responses
* Competency inference where justified

Traditional application logic handles:

* Authentication
* Authorization
* Scoring
* Competency calculations
* Gap calculations
* Persistence
* Course filtering
* Audit logging

\---

# 11\. RAG Architecture

```text
Document
  ↓
Text Extraction
  ↓
Cleaning
  ↓
Chunking
  ↓
Embeddings
  ↓
pgvector
  ↓
Similarity Search
  ↓
Relevant Context
  ↓
LLM
  ↓
Validated Response
```

RAG should support:

* Uploaded learning material
* AI assistant
* Source-grounded question generation
* Relevant content retrieval

\---

# 12\. Competency Model

Conceptual hierarchy:

```text
Competency Domain
    ↓
Competency
    ↓
Sub-Competency
    ↓
Skill / Knowledge Area
    ↓
Proficiency Level
```

Role relationship:

```text
Role
  ↓
Required Competencies
  ↓
Required Proficiency
```

Official relationship:

```text
Official
  ↓
Current Competencies
  ↓
Evidence
  ↓
Confidence
```

The competency framework must use authoritative/approved definitions wherever available.

Do not invent official competency standards and present them as authoritative.

\---

# 13\. Skill-Gap Algorithm

The basic gap is:

```text
Gap = Required Proficiency - Current Proficiency
```

Example:

```text
Required = 4
Current = 2
Gap = 2
```

The final priority may incorporate:

* Gap magnitude
* Role relevance
* Competency importance
* Evidence confidence
* Evidence quality

Conceptually:

```text
Required Competency
        -
Current Competency
        =
Skill Gap
        ↓
Priority
```

Possible priorities:

```text
HIGH
MEDIUM
LOW
```

Do not replace explainable gap calculations with an opaque ML model without architectural approval.

\---

# 14\. Recommendation Principles

Recommendations must be competency-driven.

Inputs:

```text
Official Profile
+
Role
+
Required Competencies
+
Current Competencies
+
Skill Gaps
+
Course Metadata
+
Course-Competency Mapping
+
Learning History
```

Output:

```text
Course
Recommendation Score
Priority
Reason
Related Competency
```

A recommendation should answer:

> Why is this course relevant to this official?

Example reasoning:

```text
Role requires competency X.
Current proficiency is below required level.
Course develops competency X.
Therefore course is recommended.
```

Avoid generic recommendations unrelated to competency gaps.

\---

# 15\. iGOT Integration Strategy

**Critical architectural rule: NEVER invent iGOT APIs.**

Architecture:

```text
Recommendation Engine
        ↓
IGOTService
        ↓
 ┌──────┴─────────┐
 ↓                ↓
Mock Adapter   Official Adapter
 ↓                ↓
Mock Data      Official iGOT API
```

During development:

* Use `MockIGOTAdapter`.
* Maintain realistic mock course data.
* Implement the same interface expected from the official adapter.

Official integration can only be implemented when official documentation and credentials are available.

Required information includes:

* Official API documentation
* Endpoints
* Authentication mechanism
* Credentials
* Request/response schemas
* Integration requirements

The rest of the application must not depend directly on iGOT API implementation details.

\---

# 16\. Security Principles

Security is a cross-cutting concern.

Required principles:

* Authentication for protected resources.
* RBAC for authorization.
* Resource ownership checks.
* Least privilege.
* Secrets only through environment/secret management.
* Never commit credentials.
* Validate all external input.
* Validate uploaded files.
* Enforce file-size/type restrictions.
* Isolate uploaded files.
* Never execute uploaded content.
* Protect against SQL injection.
* Protect against XSS/CSRF where applicable.
* Rate-limit sensitive endpoints where appropriate.
* Prevent cross-user AI context leakage.
* Protect against prompt injection.
* Log security-sensitive operations.
* Avoid unnecessary storage of sensitive information.

AI must not be trusted as an authorization mechanism.

\---

# 17\. Testing Rules

Every new module must include appropriate tests.

## Unit tests

Required for:

* Competency calculations
* Skill-gap calculations
* Recommendation ranking
* Quiz scoring
* Permission logic
* Role mappings

## API tests

Test:

* Valid requests
* Invalid requests
* Missing data
* Unauthorized requests
* Wrong roles
* Malformed input
* Edge cases

## AI tests

MCQ generation must validate:

* Output schema
* Number of options
* Exactly one correct answer
* Valid competency
* Valid difficulty
* Source reference
* Grounding
* Duplicate detection

AI assistant tests must validate:

* Correct user context
* No cross-user leakage
* Relevant retrieval
* Prompt-injection resistance
* No unauthorized actions

## Integration tests

Primary lifecycle:

```text
Profile
→ Assessment
→ Competency
→ Gap
→ Course
→ Recommendation
```

AI learning lifecycle:

```text
Upload
→ Extract
→ Generate
→ Validate
→ Quiz
→ Analyze
→ Update Competency
→ Recalculate Gap
```

No phase is complete until its acceptance tests pass.

\---

# 18\. Coding Conventions

General:

* Use clear, descriptive names.
* Prefer small modules and services.
* Keep domain logic separated.
* Avoid unnecessary abstraction.
* Avoid duplicated business logic.
* Use type safety wherever available.
* Validate data at boundaries.
* Keep functions focused.
* Document non-obvious architectural decisions.
* Write tests alongside significant functionality.

Backend:

* FastAPI conventions.
* Pydantic schemas for API boundaries.
* Service layer for business logic.
* Repository/data-access separation where useful.
* Database models must not be exposed directly as API contracts.

Frontend:

* TypeScript.
* Reusable components.
* Feature/domain-oriented organization.
* Avoid embedding business logic unnecessarily in UI components.

AI:

* Use structured outputs.
* Version important prompts.
* Validate every structured AI response.
* Never assume LLM output is correct.
* Keep provider-specific code isolated.

\---

# 19\. Current Development Status

**Status:** Architecture approved. Implementation has not yet begun.

Current stage:

```text
Architecture Design
        ↓
Architecture Approved
        ↓
PROJECT\_CONTEXT.md
        ↓
NEXT: Phase 0
```

\---

# 20\. Completed Modules

Currently completed at architecture/specification level:

* Overall system architecture
* Module architecture
* Database architecture
* API architecture
* AI architecture
* iGOT integration architecture
* Security architecture
* Development roadmap
* MVP definition

These are **design-complete**, not implementation-complete.

\---

# 21\. Modules Not Yet Implemented

All implementation modules are currently pending.

Priority order:

```text
1. Project Foundation
2. Authentication + RBAC
3. Database Foundation
4. Competency Framework
5. Role-to-Competency Mapping
6. Official Profile
7. Competency Assessment
8. Competency Engine
9. Skill-Gap Engine
10. iGOT Abstraction
11. iGOT Mock Adapter
12. Course-to-Competency Mapping
13. Recommendation Engine
14. Personalized Learning Path
15. Document Processing
16. Embeddings + pgvector
17. AI MCQ Generation
18. Question Validation
19. Quiz Engine
20. Quiz Analytics
21. Competency Update Engine
22. AI Assistant
23. Future Skill Intelligence
24. Admin Dashboard
25. Security Hardening
26. Testing
27. Deployment
```

\---

# 22\. Important Architectural Decisions

### Decision 1 — Competency-first

The platform is not a generic LMS.

### Decision 2 — Modular architecture

Major domains must remain independently testable.

### Decision 3 — iGOT adapter pattern

No direct coupling between business logic and iGOT APIs.

### Decision 4 — Mock before official integration

Development must work without live iGOT access.

### Decision 5 — AI provider abstraction

Do not tightly couple the platform to a single LLM provider.

### Decision 6 — Structured AI outputs

LLM output must be schema-validated.

### Decision 7 — Explainable competency updates

Competency state must retain evidence and history.

### Decision 8 — PostgreSQL + pgvector

Use PostgreSQL as the primary database and pgvector for semantic retrieval.

### Decision 9 — Deterministic critical logic

Scoring, authorization, gap calculation, and other critical business rules must not depend solely on an LLM.

### Decision 10 — SSO-ready, not invented SSO

Architecture should support future SSO integration without implementing undocumented identity systems.

\---

# 23\. Forbidden Assumptions

AI coding agents MUST NOT:

1. Invent iGOT API endpoints.
2. Invent iGOT authentication mechanisms.
3. Claim mock iGOT integration is real iGOT integration.
4. Invent government competency standards and label them official.
5. Hard-code API keys or credentials.
6. Assume a specific LLM provider is permanently available.
7. Assume paid AI subscriptions exist.
8. Replace deterministic business logic with unnecessary LLM calls.
9. Give an LLM unrestricted database access.
10. Allow AI to bypass RBAC.
11. Assume uploaded documents are trustworthy.
12. Rewrite unrelated modules when implementing a feature.
13. Change architecture without documenting the change.
14. Remove tests to make functionality pass.
15. Introduce unnecessary technologies without architectural justification.
16. Build generic LMS features that do not support competency development.
17. Claim production readiness before security and integration testing.
18. Treat AI-generated content as automatically correct.

\---

# 24\. Known Risks

## iGOT API Availability

Official integration may depend on documentation, credentials, access approval, and API availability.

**Mitigation:** Adapter architecture + mock implementation.

\---

## LLM Cost / Availability

The team has no paid AI subscriptions.

**Mitigation:**

* Provider abstraction.
* Minimize unnecessary LLM calls.
* Use deterministic logic where possible.
* Cache reusable outputs where appropriate.
* Use free/available models during development where suitable.

\---

## AI Hallucination

Generated MCQs and assistant responses may contain incorrect information.

**Mitigation:**

* RAG
* Source grounding
* Structured outputs
* Validation
* Evidence references
* Human/admin validation where appropriate

\---

## Incorrect Competency Mapping

Poor role/competency mappings can produce bad recommendations.

**Mitigation:**

* Admin-controlled mappings.
* Authoritative framework.
* Explainable recommendations.
* Validation/testing.

\---

## Data Privacy

Official profiles and learning records may contain sensitive professional information.

**Mitigation:**

* RBAC
* Resource ownership
* Minimal data collection
* Secure storage
* Audit logging
* No unnecessary AI exposure

\---

## Prompt Injection

Uploaded documents may contain malicious instructions intended to manipulate the LLM.

**Mitigation:**

* Treat retrieved content as untrusted data.
* Separate instructions from document content.
* Validate structured outputs.
* Restrict AI capabilities.
* Never allow document content to override system security rules.

\---

## Scope Creep

The project can easily become a generic LMS.

**Mitigation:**

Every feature must be evaluated against:

```text
Does this improve the competency lifecycle?
```

If not, deprioritize it.

\---

# 25\. Development Protocol for AI Coding Agents

AI coding agents must work **one milestone at a time**.

For each milestone:

```text
1. Read ARCHITECTURE.md
2. Read PROJECT\_CONTEXT.md
3. Inspect existing implementation
4. Understand current milestone
5. Implement only the requested scope
6. Preserve existing functionality
7. Add/update tests
8. Run relevant tests
9. Report files changed
10. Report tests performed
11. Report unresolved issues
12. Do not begin future milestones automatically
```

Agents must not implement the entire application in one session.

\---

# 26\. Current Next Task

The next task is:

## Phase 0 — Project Foundation

Required scope:

```text
Next.js
      ↕
FastAPI
      ↕
PostgreSQL
```

plus:

* Repository structure
* Environment configuration
* API versioning foundation
* Database connection
* Basic health checks
* Initial documentation
* Development setup

Do not implement:

* Competency engine
* iGOT integration
* AI assistant
* Quiz generation
* Recommendation engine

until Phase 0 is accepted.

\---

# 27\. Master Product Principle

The system exists to continuously answer:

```text
What competencies does this official need?
             ↓
What competencies does this official have?
             ↓
What is missing?
             ↓
What should the official learn?
             ↓
Did the learning improve competency?
             ↓
What should the official learn next?
```

Everything else is subordinate to this lifecycle.

