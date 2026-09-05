# REQUIREMENTS — AI-Enabled Competency & Learning Platform

## 1. Problem Statement

Build an AI-enabled learning platform for India's Official Statistical System that:

1. Creates a comprehensive competency profile for each official.
2. Identifies competency/skill gaps against predefined competency frameworks.
3. Recommends personalized learning through integration with the iGOT Karmayogi ecosystem.
4. Generates quizzes and MCQs from uploaded learning materials.
5. Continuously reassesses competencies and updates recommendations.
6. Provides AI-powered learning assistance.
7. Provides learner and administrator dashboards.
8. Supports future skill intelligence and workforce-level competency analysis.

---

## 2. Primary Users

### Official / Learner
The official should be able to:

- Create/view their professional profile.
- Take competency assessments.
- View current competency levels.
- View identified competency gaps.
- See personalized learning recommendations.
- Access iGOT-recommended learning resources.
- Upload learning materials.
- Generate AI-powered MCQs/quizzes from uploaded materials.
- Attempt quizzes.
- Receive instant evaluation and explanations.
- Track learning progress.
- Interact with an AI learning assistant.

### Administrator
The administrator should be able to:

- View workforce competency distribution.
- View department-level skill gaps.
- Monitor training participation and completion.
- View competency improvement.
- Identify common organizational skill gaps.
- View emerging/future skill requirements.
- Monitor recommendation and training effectiveness.

---

## 3. Core System Workflow

The primary workflow is:

PROFILE
→ COMPETENCY PROFILE
→ BASELINE ASSESSMENT
→ CURRENT COMPETENCY
→ SKILL GAP ANALYSIS
→ iGOT COURSE RETRIEVAL
→ PERSONALIZED RECOMMENDATION
→ LEARNING
→ REASSESSMENT
→ COMPETENCY UPDATE
→ NEW RECOMMENDATIONS

The uploaded-material workflow is:

UPLOAD MATERIAL
→ TEXT EXTRACTION
→ AI MCQ GENERATION
→ MCQ VALIDATION
→ QUIZ
→ EVALUATION
→ FEEDBACK
→ COMPETENCY UPDATE
→ SKILL GAP REASSESSMENT

---

## 4. Competency Profile

The system should construct a competency profile using:

- Designation
- Department
- Job role
- Current assignment
- Educational qualifications
- Work experience
- Previous training
- Assessment results
- Learning history
- Quiz performance

The competency profile should not depend only on self-declared skills.

---

## 5. Competency Domains

The initial competency framework should include:

### Statistical Competencies

- Survey Design
- Sampling
- National Accounts
- Price Statistics
- Labour Statistics
- Agricultural Statistics
- Industrial Statistics
- SDG Indicators
- Metadata Standards
- Data Quality Frameworks

### Technical Competencies

- Python
- R
- SQL
- Stata
- SPSS
- SAS
- GIS
- Data Visualization
- AI/ML
- Cloud Computing
- APIs
- Open Data

### Digital Governance

- Cybersecurity
- Data Privacy
- Digital Signatures
- Government Cloud
- Digital Public Infrastructure

### Behavioural / Managerial

- Leadership
- Communication
- Project Management
- Ethics
- Decision Making
- Change Management

The framework must be designed so additional competencies can be added later without changing the core architecture.

---

## 6. Competency Levels

Use a standardized competency scale initially:

0 — No Evidence
1 — Beginner
2 — Basic
3 — Intermediate
4 — Advanced
5 — Expert

The system must distinguish between:

### Required Competency

The competency level expected for a particular role/designation.

### Current Competency

The competency level demonstrated by the official based on available evidence.

### Competency Gap

The difference between required competency and current competency.

Example:

Required Python level = 4
Current Python level = 2

Gap = 2

The competency-gap calculation must be deterministic and implemented in backend logic rather than delegated entirely to an LLM.

---

## 7. Evidence-Based Competency Scoring

Competency scores should eventually use multiple evidence sources:

- Profile information
- Previous training
- Assessment results
- Quiz results
- Learning history

The architecture should allow additional evidence sources to be added later.

The system should maintain evidence for why a competency score was assigned.

Example:

Python = Level 2

Evidence:
- Completed Python training
- Baseline assessment score: 58%
- Recent quiz score: 61%

This makes the competency assessment explainable.

---

## 8. Skill Gap Analysis

For every official:

1. Identify competencies required by their role.
2. Determine their current competency.
3. Calculate the competency gap.
4. Prioritize gaps.

Gap prioritization should consider:

- Size of gap
- Importance of competency to role
- Departmental priority
- Future relevance
- Existing learning history

The result should identify:

- Critical gaps
- High-priority gaps
- Moderate gaps
- Low-priority gaps

---

## 9. iGOT Karmayogi Integration

The platform must be designed for integration with the iGOT Karmayogi ecosystem.

The system should support:

- Retrieving relevant learning/course catalogue information.
- Mapping courses to competencies.
- Recommending courses based on competency gaps.
- Tracking learning/enrolment/completion where supported by official APIs.
- Updating competency evidence after learning.

IMPORTANT:

Do NOT invent iGOT API endpoints, authentication methods, request formats, or response formats.

The system must use an abstraction layer:

Frontend
→ Backend
→ iGOT Service
→ iGOT Provider
→ Official iGOT API

The provider architecture should support:

- Mock provider for development/testing.
- Real iGOT provider for official API integration.

The real provider must only be implemented using official iGOT API documentation and credentials.

Configuration should allow switching providers without rewriting the recommendation system.

---

## 10. Course Recommendation Engine

Recommendations should be based on:

- Competency gap
- Role relevance
- Department relevance
- Course competency coverage
- Course difficulty
- Previous learning
- Learning history
- Future skill relevance

The initial recommendation engine should use deterministic scoring/ranking.

LLMs may be used to:

- Explain recommendations.
- Perform semantic matching.
- Extract competency information from course descriptions.

The LLM must not be solely responsible for numerical recommendation ranking.

Each recommendation should explain:

- Which competency gap it addresses.
- Why the course is relevant.
- Expected competency improvement.

---

## 11. Personalized Learning Path

The system should generate a learning path based on priority gaps.

Example:

Official:
Statistical Officer

Gap:
Python = 2 levels
SQL = 1 level
Data Visualization = 2 levels

Learning path:

1. Python fundamentals
2. Python for statistical analysis
3. SQL fundamentals
4. Data visualization
5. Practical assessment

The learning path should be dynamically recalculated when competency evidence changes.

---

## 12. AI Quiz / MCQ Generation

The system should allow users to upload learning materials.

Initial supported formats:

- PDF
- DOCX
- PPTX

Workflow:

Upload
→ Extract text
→ Clean/chunk text
→ Generate MCQs
→ Validate MCQs
→ Store questions
→ User attempts quiz
→ Evaluate answers
→ Generate explanations
→ Update competency evidence

Each MCQ should ideally contain:

- Question
- Four options
- Correct answer
- Explanation
- Difficulty
- Competency tag
- Source/reference text

The system should avoid generating questions unsupported by the uploaded material.

---

## 13. Quiz Evaluation

The quiz system should provide:

- Score
- Correct/incorrect answers
- Explanations
- Competency-wise performance
- Difficulty-wise performance
- Feedback
- Recommended next learning action

Quiz results should become evidence for competency scoring.

---

## 14. AI Learning Assistant

The platform should include an AI-powered learning assistant.

The assistant should eventually be able to:

- Explain competency concepts.
- Answer questions about uploaded learning materials.
- Explain incorrect quiz answers.
- Recommend what to study next.
- Provide examples.
- Help users understand difficult topics.

The assistant should preferably use Retrieval-Augmented Generation (RAG) over approved learning materials.

It should not fabricate information when the answer cannot be supported by available material.

---

## 15. Future Skill Intelligence

The platform should provide a future-skills capability.

It should identify potentially important emerging competencies based on:

- Role
- Department
- Existing competency gaps
- Technology trends
- Organizational priorities
- Emerging statistical practices

Future-skill outputs must be presented as recommendations/scenario-based intelligence rather than guaranteed predictions.

---

## 16. Learner Dashboard

The learner dashboard should show:

- Competency profile
- Competency levels
- Skill gaps
- Gap priorities
- Recommended courses
- Learning path
- Learning progress
- Quiz performance
- Competency improvement
- AI assistant
- Future skill recommendations

---

## 17. Administrator Dashboard

The administrator dashboard should show:

- Total officials
- Competency distribution
- Department-level competency gaps
- Role-level competency gaps
- Training participation
- Training completion
- Competency improvement
- Common skill gaps
- Emerging skill requirements
- Workforce competency heatmap

---

## 18. Security

The system should be designed with:

- Role-Based Access Control (RBAC)
- Secure authentication architecture
- Secure API communication
- Input validation
- File upload validation
- Protection against malicious uploads
- Secure storage of sensitive information
- No API keys or secrets committed to Git
- Environment variables for secrets
- Audit logging where appropriate

The architecture should be SSO-ready.

Do not claim actual government security compliance unless it has been verified.

---

## 19. Technology Stack

Preferred initial stack:

### Frontend
- Next.js
- React
- TypeScript
- Tailwind CSS

### Backend
- Python
- FastAPI

### Database
- PostgreSQL
- pgvector where semantic search is required

### AI / ML
- Python
- LLM abstraction layer
- scikit-learn where traditional ML is appropriate
- RAG for learning assistant

### Document Processing
- PyMuPDF
- python-docx
- python-pptx

### Development
- Git
- GitHub
- VS Code
- Docker where useful

The architecture should avoid unnecessary technologies.

---

## 20. AI Architecture

AI functionality must be separated behind service interfaces.

Example:

AIService
→ AIProvider

This allows different models/providers to be used without rewriting the application.

Do not hard-code the entire application around one LLM provider.

AI should be used where it adds value:

- NLP
- Semantic matching
- MCQ generation
- Explanations
- RAG assistant
- Profile information extraction

Deterministic application logic should remain normal backend code.

---

## 21. MVP Priority

The MVP must prioritize working end-to-end functionality over a large number of disconnected features.

### Priority 1 — Core

- Official profile
- Competency framework
- Role → competency mapping
- Baseline assessment
- Competency scoring
- Skill-gap analysis

### Priority 2 — iGOT

- iGOT integration abstraction
- Course catalogue
- Course → competency mapping
- Personalized recommendations

### Priority 3 — Assessment Intelligence

- PDF/DOCX/PPTX upload
- Text extraction
- AI MCQ generation
- Quiz
- Evaluation
- Competency update

### Priority 4 — User Experience

- Learner dashboard
- Admin dashboard
- Learning path
- AI assistant

### Priority 5 — Advanced

- Future skill intelligence
- Advanced analytics
- SSO integration
- Predictive workforce analytics
- Multilingual support

---

## 22. Primary SIH Demonstration Flow

The final MVP demonstration should show one complete official journey:

1. Admin creates an official.
2. Official profile is created.
3. System generates competency profile.
4. Official takes baseline assessment.
5. System calculates current competency.
6. System compares current competency with role requirements.
7. Skill gaps are identified and prioritized.
8. System retrieves relevant iGOT learning opportunities.
9. Personalized learning path is generated.
10. Official studies learning material.
11. Official uploads learning material.
12. AI generates competency-tagged MCQs.
13. Official takes quiz.
14. System evaluates performance.
15. Competency score is updated.
16. Skill gaps are recalculated.
17. Recommendations change accordingly.
18. Admin dashboard reflects workforce competency improvement.

This closed-loop learning cycle is a central differentiator of the platform.

---

## 23. Important Development Rules

1. Do not invent external API specifications.
2. Do not hard-code secrets.
3. Do not use an LLM for deterministic business calculations.
4. Keep AI functionality behind service interfaces.
5. Keep iGOT integration behind an adapter/provider interface.
6. Build incrementally.
7. Avoid unnecessary technologies.
8. Every major feature must have tests.
9. Preserve existing functionality when adding features.
10. Do not make large architectural changes without documenting the reason.
11. Maintain API contracts between frontend and backend.
12. Keep database schema migrations version controlled.
13. Make the system runnable locally.
14. The MVP should work even when external services are unavailable by using controlled mock providers.
15. Clearly label mocked integrations during development/demo preparation.

---

## 24. Definition of Done for MVP

The MVP is considered successful when a demo user can:

Create profile
→ Take assessment
→ Receive competency profile
→ See skill gaps
→ Receive personalized iGOT learning recommendations
→ Upload learning material
→ Generate MCQs
→ Take quiz
→ Receive feedback
→ Have competency updated
→ See updated recommendations
→ View progress on dashboard.

An administrator should additionally be able to view organization-level competency and skill-gap information.