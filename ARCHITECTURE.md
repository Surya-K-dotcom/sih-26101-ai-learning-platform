# SIH 2026 — AI-Enabled Competency Learning Platform

**Problem Statement ID:** 26101  
**Project:** AI-enabled learning platform for competency-gap identification and personalized capacity building in India's Official Statistical System  
**Architecture Version:** 1.0  
**Status:** Initial Architecture  
**Primary Role:** Lead Architect / Project Manager

\---

# 1\. Project Vision

The platform is designed to identify competency gaps of officials in India's Official Statistical System and provide personalized learning recommendations through integration with the **iGOT Karmayogi ecosystem**.

The system must **not become a generic Learning Management System (LMS)**.

Its primary purpose is **competency intelligence and personalized capacity building**.

The central lifecycle is:

```text
PROFILE
    ↓
ASSESS
    ↓
COMPETENCY
    ↓
SKILL GAP
    ↓
iGOT COURSE
    ↓
RECOMMEND
    ↓
LEARN
    ↓
ASSESS
    ↓
UPDATE COMPETENCY
    ↓
RECOMMEND AGAIN
```

A second learning loop handles uploaded learning materials:

```text
UPLOAD MATERIAL
    ↓
EXTRACT
    ↓
GENERATE MCQs
    ↓
VALIDATE
    ↓
QUIZ
    ↓
ANALYZE PERFORMANCE
    ↓
UPDATE COMPETENCY
    ↓
RECOMMEND AGAIN
```

\---

# 2\. Architectural Principles

## 2.1 Competency-first architecture

Every major feature should contribute to the competency lifecycle.

The platform should answer:

1. Who is the official?
2. What competencies are required for their role?
3. What competencies do they currently possess?
4. Where are the gaps?
5. What training can address those gaps?
6. What should they learn next?
7. Did learning improve their competencies?
8. What should they learn next after reassessment?

\---

## 2.2 Not a generic LMS

The system should prioritize:

* Competency profiling
* Competency assessment
* Skill-gap analysis
* Personalized recommendations
* Competency-aware learning
* Competency-aware assessments
* Automatic competency updates

Generic LMS features should only be implemented if they support these objectives.

\---

## 2.3 AI should not control deterministic business logic

Use AI for:

* Natural-language understanding
* Semantic matching
* Document understanding
* MCQ generation
* RAG
* AI assistant
* Competency inference where appropriate

Use deterministic application logic for:

* Authentication
* Authorization
* RBAC
* Quiz scoring
* Competency calculations
* Skill-gap calculations
* Database operations
* Course filtering
* Audit logging
* Security controls

\---

## 2.4 Explainability

Every important AI-generated or competency-related result should be explainable.

For example:

```text
Recommendation:
"Course X is recommended because:

- Your role requires Data Quality.
- Required proficiency: 4
- Current proficiency: 2
- Identified gap: 2
- Course X develops Data Quality competency."
```

\---

## 2.5 iGOT integration abstraction

The system must not invent or assume iGOT APIs.

Until official iGOT documentation, endpoints, authentication mechanisms, and credentials are available:

```text
Mock iGOT Adapter
```

will be used.

When official integration details become available:

```text
Official iGOT Adapter
```

can replace the mock implementation without changing the recommendation engine or frontend.

\---

# 3\. Technology Stack

## Frontend

```text
Next.js
React
TypeScript
```

Responsibilities:

* User interface
* Official dashboard
* Admin dashboard
* Assessments
* Quiz interface
* Learning paths
* Recommendations
* AI assistant
* Authentication UI

\---

## Backend

```text
Python
FastAPI
```

Responsibilities:

* REST APIs
* Authentication
* RBAC
* Business logic
* Competency engine
* Assessment engine
* Recommendation engine
* Learning path engine
* Document processing
* AI orchestration
* iGOT integration layer

\---

## Database

```text
PostgreSQL
```

Used for:

* Users
* Profiles
* Competencies
* Assessments
* Courses
* Recommendations
* Learning paths
* Quizzes
* Documents
* Audit logs
* AI conversations

\---

## Vector Search

```text
pgvector
```

Used for:

* Document embeddings
* Semantic search
* RAG
* Semantic course/competency matching where required

\---

## AI

```text
LLM API
Structured Outputs
Embedding Model
```

AI providers should remain configurable rather than hard-coded into business logic.

\---

## Machine Learning

```text
Python
scikit-learn
```

Use only where conventional ML provides meaningful value.

Do not introduce ML merely for the sake of using ML.

\---

## Document Processing

```text
PyMuPDF
python-docx
python-pptx
```

Supported initial formats:

```text
PDF
DOCX
PPTX
```

\---

# 4\. High-Level System Architecture

```text
                         ┌─────────────────────────┐
                         │         USERS           │
                         │                         │
                         │ Officials / Admins      │
                         └────────────┬────────────┘
                                      │
                                      ▼
                    ┌─────────────────────────────────┐
                    │        NEXT.JS FRONTEND         │
                    │                                 │
                    │ Official Dashboard              │
                    │ Assessment                      │
                    │ Competencies                    │
                    │ Skill Gaps                      │
                    │ Recommendations                 │
                    │ Learning Path                   │
                    │ Quiz                            │
                    │ AI Assistant                    │
                    │ Admin Dashboard                 │
                    └───────────────┬─────────────────┘
                                    │ REST / JSON
                                    ▼
                    ┌─────────────────────────────────┐
                    │          FASTAPI BACKEND        │
                    │                                 │
                    │ Authentication / RBAC           │
                    │ Official Profile                 │
                    │ Competency Engine                │
                    │ Assessment Engine                │
                    │ Skill Gap Engine                 │
                    │ Recommendation Engine            │
                    │ Learning Path Engine             │
                    │ Document Processing              │
                    │ Quiz Engine                      │
                    │ AI Assistant                     │
                    │ iGOT Integration                 │
                    └───────┬───────────┬─────────────┘
                            │           │
                 ┌──────────┘           └──────────┐
                 ▼                                 ▼
       ┌──────────────────────┐         ┌──────────────────────┐
       │      PostgreSQL      │         │       AI / ML        │
       │                      │         │                      │
       │ Users                │         │ LLM                  │
       │ Profiles             │         │ Embeddings           │
       │ Competencies         │         │ RAG                  │
       │ Assessments          │         │ MCQ Generation       │
       │ Skill Gaps           │         │ AI Assistant         │
       │ Courses              │         │ ML Models            │
       │ Learning Paths       │         └──────────────────────┘
       │ Quizzes              │
       │ Documents            │
       │ pgvector             │
       │ Audit Logs            │
       └──────────┬───────────┘
                  │
                  ▼
       ┌──────────────────────────┐
       │   iGOT INTEGRATION LAYER │
       │                          │
       │ iGOT Service Interface   │
       │ Mock Adapter             │
       │ Official Adapter         │
       └────────────┬─────────────┘
                    │
                    ▼
             ┌──────────────┐
             │ iGOT Karmayogi│
             │ Ecosystem     │
             └──────────────┘
```

\---

# 5\. Module Architecture

The backend should be organized into independent domain modules.

\---

## 5.1 Identity and Access Module

Responsibilities:

* Authentication
* Authorization
* RBAC
* Session/token management
* SSO-ready architecture

Initial roles:

```text
OFFICIAL
ADMIN
```

Future roles:

```text
TRAINING\_MANAGER
CONTENT\_ADMIN
SYSTEM\_ADMIN
ASSESSOR
```

\---

# 5.2 Official Profile Module

Maintains the professional profile of every official.

Profile information includes:

```text
Designation
Department
Job Role
Current Assignment
Educational Qualifications
Work Experience
Previous Trainings
Current Competencies
```

The official profile is the foundation for personalization.

\---

# 5.3 Competency Framework Module

Maintains the competency framework.

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

The framework should use authoritative/approved competency definitions rather than arbitrary invented competencies.

\---

# 5.4 Role-to-Competency Mapping Module

Maps professional roles to required competencies.

```text
Role
  ↓
Required Competencies
  ↓
Required Proficiency
```

Example:

```text
Statistical Officer
    │
    ├── Data Quality
    ├── Statistical Methods
    ├── Data Analysis
    ├── Official Statistics
    └── Data Visualization
```

Mappings should include:

```text
Role
Competency
Required Proficiency
Importance
```

\---

# 5.5 Competency Assessment Engine

Determines the current competency level of an official.

Potential evidence sources:

```text
Initial Assessment
Self Assessment
Quiz Performance
Training Completion
Uploaded-Material Assessments
Future Work/Project Evidence
```

The engine should produce:

```text
Competency
Current Proficiency
Confidence
Evidence
Timestamp
```

\---

# 5.6 Skill Gap Engine

Compares required competency against current competency.

Basic model:

```text
Required Competency
        -
Current Competency
        =
Competency Gap
```

Example:

```text
Required Proficiency = 4
Current Proficiency  = 2

Gap = 2
```

The eventual gap score may also consider:

```text
Gap Magnitude
Competency Importance
Role Relevance
Confidence
Evidence Quality
```

Gap priority:

```text
HIGH
MEDIUM
LOW
```

\---

# 5.7 iGOT Integration Module

The iGOT integration must be isolated behind an adapter interface.

```text
Recommendation Engine
        │
        ▼
   iGOT Service
        │
   ┌────┴─────┐
   ▼          ▼
Mock Adapter  Official Adapter
   │          │
   ▼          ▼
Mock Data   Official iGOT API
```

The application must not directly depend on undocumented iGOT endpoints.

\---

# 5.8 Course-to-Competency Mapping Module

Maps learning resources to competencies.

```text
Course
  ↓
Competencies Developed
  ↓
Expected Proficiency Improvement
```

Example:

```text
Course:
Introduction to Statistical Data Quality

Competencies:
- Data Quality
- Data Validation
- Data Management
```

\---

# 5.9 Recommendation Engine

Inputs:

```text
Official Profile
Role
Required Competencies
Current Competencies
Skill Gaps
Available Courses
Course-Competency Mapping
Previous Learning
```

Output:

```text
Recommended Courses
Recommendation Score
Recommendation Reason
Priority
```

Recommendations should be explainable.

\---

# 5.10 Personalized Learning Path Engine

Converts recommendations into an ordered learning journey.

Example:

```text
Data Quality
    ↓
Foundation Course
    ↓
Intermediate Course
    ↓
Advanced Course
    ↓
Assessment
    ↓
Competency Update
```

The learning path should adapt after reassessment.

\---

# 5.11 Document Intelligence Module

Pipeline:

```text
Upload
   ↓
File Validation
   ↓
Text Extraction
   ↓
Cleaning
   ↓
Chunking
   ↓
Metadata Extraction
   ↓
Embedding
   ↓
pgvector
```

Supported formats:

```text
PDF
DOCX
PPTX
```

\---

# 5.12 AI Question Generation Module

Pipeline:

```text
Learning Material
      ↓
Relevant Content Chunks
      ↓
LLM
      ↓
Structured MCQs
      ↓
Schema Validation
      ↓
Duplicate Detection
      ↓
Source Verification
      ↓
Competency Mapping
      ↓
Question Bank
```

Each question should contain conceptually:

```text
Question
Options
Correct Answer
Explanation
Difficulty
Competency
Source Reference
Validation Status
```

\---

# 5.13 Quiz Engine

Responsibilities:

* Quiz creation
* Question selection
* Quiz attempts
* Answer submission
* Scoring
* Performance analysis
* Competency mapping
* Feedback

The quiz should provide more than an overall score.

Example:

```text
Overall Score: 70%

Data Quality:        65%
Data Validation:     85%
Error Detection:     45%
Statistical Methods: 80%
```

\---

# 5.14 Competency Update Engine

Quiz and assessment evidence should update competency state.

```text
Quiz Performance
      ↓
Competency Evidence
      ↓
Competency Update
      ↓
Skill Gap Recalculation
      ↓
New Recommendation
```

Competency updates should maintain evidence history rather than simply overwriting old scores.

\---

# 5.15 Competency-Aware AI Assistant

The assistant should not function as a generic chatbot.

Its context should include:

```text
Official Profile
Current Competencies
Required Competencies
Skill Gaps
Learning Path
Relevant Courses
Uploaded Materials
Learning History
```

Example query:

```text
"What should I learn next?"
```

The response should be based on the user's actual competency gaps.

\---

# 5.16 Future Skill Intelligence Module

Future skill intelligence identifies emerging competency requirements.

Initial implementation may use an admin-managed dataset:

```text
Emerging Skill
Related Competency
Relevant Roles
Importance
Expected Future Relevance
```

Future versions may use:

```text
Historical Training Data
Role Evolution
Emerging Technologies
Statistical Trends
ML Forecasting
```

Advanced prediction is not required for the first MVP.

\---

# 5.17 Official Dashboard

The official dashboard should answer:

```text
Who am I professionally?
What competencies do I have?
What competencies are required?
Where are my gaps?
What should I learn?
How am I progressing?
```

Dashboard components:

```text
Professional Profile
Competency Profile
Skill Gap Visualization
Recommended Courses
Learning Path
Assessment History
Quiz Performance
Competency Progress
AI Assistant
```

\---

# 5.18 Administrator Dashboard

Admin functions:

```text
User Management
Role Management
Competency Framework Management
Role-Competency Mapping
Course Management
Course-Competency Mapping
Question Management
Assessment Management
Document Management
Analytics
Audit Logs
```

\---

# 6\. Database Architecture

Database:

```text
PostgreSQL
```

Vector extension:

```text
pgvector
```

\---

## 6.1 Core entities

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

\---

# 7\. Core Database Relationships

## Official competency lifecycle

```text
User
 │
 └── Official Profile
        │
        ├── Role
        │
        └── Official Competencies
                  │
                  ▼
              Skill Gaps
                  │
                  ▼
          Course Recommendations
                  │
                  ▼
             Learning Path
                  │
                  ▼
              Assessment
                  │
                  ▼
        Competency Evidence
                  │
                  ▼
        Updated Competency
```

\---

## Document lifecycle

```text
Document
    │
    └── Document Chunks
             │
             └── Embeddings
                    │
                    └── pgvector
```

\---

## Important competency data rule

Do not store competency state as an unexplained number.

Maintain:

```text
Score
Source
Evidence
Assessment
Confidence
Timestamp
```

This allows the system to explain how a competency score was obtained.

\---

# 8\. API Architecture

Use versioned REST APIs:

```text
/api/v1/
```

\---

## 8.1 Authentication APIs

```text
POST /api/v1/auth/login
POST /api/v1/auth/logout
GET  /api/v1/auth/me
```

SSO-ready architecture:

```text
/api/v1/auth/sso/\*
```

Actual SSO integration should only be implemented using verified identity-provider documentation.

\---

# 8.2 Official APIs

```text
GET /api/v1/officials/me
PUT /api/v1/officials/me

GET /api/v1/officials/me/competencies
GET /api/v1/officials/me/skill-gaps
GET /api/v1/officials/me/recommendations
GET /api/v1/officials/me/learning-path
```

\---

# 8.3 Competency APIs

```text
GET /api/v1/competencies
GET /api/v1/competencies/{id}

GET /api/v1/roles/{id}/competencies
```

Admin:

```text
POST   /api/v1/admin/competencies
PUT    /api/v1/admin/competencies/{id}
DELETE /api/v1/admin/competencies/{id}
```

\---

# 8.4 Assessment APIs

```text
POST /api/v1/assessments
GET  /api/v1/assessments/{id}
POST /api/v1/assessments/{id}/submit
GET  /api/v1/assessments/{id}/result
```

\---

# 8.5 Skill Gap APIs

```text
GET  /api/v1/competency-gaps/me
POST /api/v1/competency-gaps/recalculate
```

\---

# 8.6 iGOT APIs

Application-facing APIs:

```text
GET /api/v1/igot/courses
GET /api/v1/igot/courses/{id}
GET /api/v1/igot/courses/search
```

Internal architecture:

```text
IGOTService
    │
    ├── MockIGOTAdapter
    │
    └── OfficialIGOTAdapter
```

No undocumented endpoint should be implemented.

\---

# 8.7 Recommendation APIs

```text
GET  /api/v1/recommendations/me
POST /api/v1/recommendations/generate
```

\---

# 8.8 Learning Path APIs

```text
GET  /api/v1/learning-path/me
POST /api/v1/learning-path/generate
GET  /api/v1/learning-path/{id}
```

\---

# 8.9 Document APIs

```text
POST /api/v1/documents/upload
GET  /api/v1/documents
GET  /api/v1/documents/{id}
```

\---

# 8.10 Question Generation APIs

```text
POST /api/v1/documents/{id}/generate-questions

GET  /api/v1/question-banks/{id}

POST /api/v1/question-banks/{id}/validate
```

\---

# 8.11 Quiz APIs

```text
POST /api/v1/quizzes
GET  /api/v1/quizzes/{id}

POST /api/v1/quizzes/{id}/start
POST /api/v1/quizzes/{id}/submit

GET /api/v1/quizzes/{id}/result
```

\---

# 8.12 AI Assistant APIs

```text
POST /api/v1/assistant/chat

GET /api/v1/assistant/conversations
GET /api/v1/assistant/conversations/{id}
```

\---

# 9\. AI Architecture

The AI layer should be an independent service/orchestration layer.

```text
Application
    ↓
AI Service
    ↓
Prompt / Context Builder
    ↓
LLM Provider
    ↓
Structured Output
    ↓
Validation
    ↓
Application Logic
```

AI providers should be configurable.

The application must not become tightly coupled to one provider.

\---

# 10\. RAG Architecture

```text
Uploaded Document
       ↓
Text Extraction
       ↓
Text Cleaning
       ↓
Chunking
       ↓
Embedding Model
       ↓
pgvector
       ↓
Similarity Search
       ↓
Relevant Context
       ↓
LLM
       ↓
Response
```

RAG will primarily support:

* Uploaded learning material
* AI assistant
* Question generation
* Source-grounded responses

\---

# 11\. MCQ Generation Architecture

```text
Document
   ↓
Extracted Content
   ↓
Relevant Chunks
   ↓
LLM
   ↓
Structured MCQs
   ↓
Schema Validation
   ↓
Content Validation
   ↓
Duplicate Detection
   ↓
Source Verification
   ↓
Competency Mapping
   ↓
Question Bank
```

The generated MCQ must remain grounded in the uploaded material.

\---

# 12\. AI Output Validation

Never trust raw LLM output.

Every structured AI result should pass:

```text
Schema Validation
       ↓
Business Validation
       ↓
Content Validation
       ↓
Persistence
```

For MCQs:

```text
Valid question
Valid options
Exactly one correct answer
Valid competency
Valid difficulty
Source reference present
No duplicate
```

\---

# 13\. Recommendation Architecture

Recommendation flow:

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
       ↓
Recommendation Engine
       ↓
Ranked Courses
       ↓
Explainable Recommendations
```

\---

# 14\. iGOT Integration Architecture

## Development

```text
Application
    ↓
IGOTService
    ↓
MockIGOTAdapter
    ↓
Local Mock Course Dataset
```

This enables development without requiring live iGOT access.

\---

## Production

```text
Application
    ↓
IGOTService
    ↓
OfficialIGOTAdapter
    ↓
Official iGOT APIs
```

The official adapter must only be implemented after receiving:

```text
Official API Documentation
API Endpoints
Authentication Specification
Required Credentials
Request/Response Schemas
Integration Requirements
```

No endpoint, credential, or API behavior should be invented.

\---

# 15\. Security Architecture

Security must be implemented throughout development.

\---

## 15.1 Authentication

Development:

```text
JWT/session-based authentication
```

Production:

```text
Identity Provider
       ↓
SSO
       ↓
Application
```

The architecture must remain SSO-ready.

\---

# 15.2 RBAC

Every protected request should follow:

```text
Authentication
      ↓
Authorization
      ↓
Resource Ownership
      ↓
Operation
```

Example:

An official cannot:

```text
Modify competency framework
Access admin APIs
View another official's private data
Modify another official's competency profile
```

\---

# 15.3 Data protection

Protect:

```text
Personal Information
Professional Information
Assessment Results
Competency Profiles
Learning History
Uploaded Documents
AI Conversations
```

\---

# 15.4 File security

Uploaded documents must undergo:

```text
Extension Validation
MIME Validation
File Size Validation
Safe Filename Handling
Isolated Storage
Safe Processing
```

Uploaded files must never be executed.

\---

# 15.5 AI security

Protect against:

```text
Prompt Injection
Indirect Prompt Injection
Data Leakage
Cross-user Context Leakage
Hallucination
Malicious Documents
Unauthorized Tool Access
```

The AI assistant must only access data belonging to the authenticated user unless an explicitly authorized admin operation allows otherwise.

\---

# 15.6 Audit Logging

Important actions should be logged:

```text
Login
Profile Modification
Competency Modification
Assessment
Quiz Attempt
Document Upload
AI Generation
Course Mapping
Recommendation Generation
Administrative Changes
```

Audit logs should contain sufficient information for security and debugging without unnecessarily storing sensitive content.

\---

# 16\. Development Phases

The project must be developed incrementally.

\---

## Phase 0 — Foundation

Build:

```text
Repository
Frontend
Backend
PostgreSQL
Environment Configuration
Basic Frontend ↔ Backend Communication
Backend ↔ Database Communication
```

Acceptance criterion:

```text
Next.js → FastAPI → PostgreSQL
```

works successfully.

\---

## Phase 1 — Authentication + RBAC

Build:

```text
Users
Authentication
Roles
Authorization
Official/Admin separation
```

Acceptance:

```text
Official → Official APIs = allowed
Official → Admin APIs = denied
Admin → Admin APIs = allowed
```

\---

## Phase 2 — Competency Framework

Build:

```text
Competency Domains
Competencies
Competency Levels
Roles
Role → Competency Mapping
```

Acceptance:

```text
Role → Required Competencies
```

works.

\---

## Phase 3 — Official Profile

Build:

```text
Official Profile
Designation
Department
Role
Education
Experience
Previous Training
```

Dashboard:

```text
My Profile
My Role
My Competencies
```

\---

## Phase 4 — Assessment + Skill Gap Engine

Build:

```text
Assessment
Scoring
Current Competency
Required Competency
Gap Calculation
Gap Prioritization
```

First major milestone:

```text
PROFILE
    ↓
ASSESS
    ↓
COMPETENCY
    ↓
SKILL GAP
```

\---

## Phase 5 — iGOT Abstraction + Mock

Build:

```text
IGOTService
MockIGOTAdapter
Course Dataset
Course Retrieval
Course-Competency Mapping
```

Acceptance:

```text
Skill Gap
    ↓
Relevant Courses
```

\---

## Phase 6 — Recommendations + Learning Path

Build:

```text
Recommendation Engine
Ranking
Recommendation Explanation
Learning Path
Learning Progress
```

The first complete primary loop:

```text
PROFILE
→ ASSESS
→ COMPETENCY
→ GAP
→ iGOT COURSE
→ RECOMMEND
→ LEARN
```

\---

## Phase 7 — Document Processing

Build:

```text
PDF Processing
DOCX Processing
PPTX Processing
Text Extraction
Cleaning
Chunking
Embeddings
pgvector
```

Each file type should be independently tested.

\---

## Phase 8 — AI MCQ + Quiz

Build:

```text
Document
    ↓
AI Question Generation
    ↓
Validation
    ↓
Question Bank
    ↓
Quiz
    ↓
Evaluation
```

\---

## Phase 9 — Competency Update Loop

Connect:

```text
Quiz
 ↓
Performance
 ↓
Competency Evidence
 ↓
Competency Update
 ↓
Skill Gap Recalculation
 ↓
New Recommendation
```

Second major milestone:

```text
UPLOAD
→ EXTRACT
→ GENERATE
→ VALIDATE
→ QUIZ
→ ANALYZE
→ UPDATE
```

\---

## Phase 10 — AI Assistant

Build only after competency and document systems are operational.

Assistant context:

```text
Profile
Competencies
Skill Gaps
Learning Path
Courses
Uploaded Documents
Learning History
```

\---

## Phase 11 — Future Skill Intelligence

Build:

```text
Future Skills
Emerging Competencies
Role Evolution
Future Gap Analysis
```

Keep the first implementation explainable and controlled.

\---

## Phase 12 — Production Hardening

Perform:

```text
Security Review
API Validation
Rate Limiting
Error Handling
Logging
Database Indexing
Performance Testing
File Security
AI Safety
UI Polish
End-to-End Testing
Deployment
```

\---

# 17\. MVP Scope

## Must Have

```text
✓ Official Competency Profile
✓ Competency Framework
✓ Role-to-Competency Mapping
✓ Competency Assessment
✓ Competency Scoring
✓ Skill-Gap Analysis
✓ iGOT Integration Abstraction
✓ iGOT Development Mock
✓ Course Retrieval
✓ Course-to-Competency Mapping
✓ Personalized Recommendations
✓ Personalized Learning Path
✓ PDF/DOCX/PPTX Processing
✓ AI MCQ Generation
✓ AI Quiz Generation
✓ Quiz Evaluation
✓ Real-Time Feedback
✓ Automatic Competency Updates
✓ Competency-Aware AI Assistant
✓ Official Dashboard
✓ Administrator Dashboard
✓ RBAC
✓ SSO-Ready Architecture
✓ Security Fundamentals
```

\---

## Simplified / Future Features

These should not delay the core system:

```text
Advanced Future Skill Prediction
Advanced ML Forecasting
Complex Analytics
Mobile Application
Advanced Notification System
Collaborative Learning
Advanced Gamification
```

\---

# 18\. Testing Strategy

Testing should occur continuously rather than at the end.

\---

## 18.1 Unit Testing

Test:

```text
Competency Calculations
Skill-Gap Calculations
Recommendation Ranking
Quiz Scoring
Role Mapping
Permission Checks
```

Example:

```text
Required = 4
Current = 2
Expected Gap = 2
```

\---

## 18.2 API Testing

For every API test:

```text
Valid Request
Invalid Request
Unauthorized Request
Wrong Role
Missing Data
Malformed Data
Edge Cases
```

\---

## 18.3 Database Testing

Verify:

```text
Foreign Keys
Constraints
Unique Fields
Indexes
Relationships
Cascade Behavior
Vector Storage
```

\---

## 18.4 AI Testing

### MCQ tests

Verify:

```text
Valid JSON
Correct number of options
Exactly one correct answer
Valid competency
Valid difficulty
Source reference
No duplicate questions
Question grounded in source
```

### AI Assistant tests

Verify:

```text
Correct user context
No cross-user data leakage
Relevant retrieval
Source grounding
Prompt-injection resistance
No unauthorized actions
```

\---

## 18.5 Integration Testing

### Primary flow

```text
Create Official
      ↓
Assign Role
      ↓
Assessment
      ↓
Competency Calculation
      ↓
Skill Gap
      ↓
Course Retrieval
      ↓
Recommendation
```

### AI learning flow

```text
Upload PDF
      ↓
Extract
      ↓
Generate Questions
      ↓
Validate
      ↓
Create Quiz
      ↓
Attempt Quiz
      ↓
Evaluate
      ↓
Update Competency
      ↓
Recalculate Gap
```

\---

# 19\. End-to-End SIH Demonstration Flow

The final demonstration should follow this sequence:

```text
LOGIN
   ↓
OFFICIAL PROFILE
   ↓
INITIAL ASSESSMENT
   ↓
COMPETENCY PROFILE
   ↓
SKILL GAP ANALYSIS
   ↓
iGOT COURSE RETRIEVAL
   ↓
PERSONALIZED RECOMMENDATION
   ↓
PERSONALIZED LEARNING PATH
   ↓
UPLOAD LEARNING MATERIAL
   ↓
AI GENERATES MCQs
   ↓
VALIDATION
   ↓
QUIZ
   ↓
REAL-TIME FEEDBACK
   ↓
COMPETENCY UPDATE
   ↓
SKILL GAP RECALCULATION
   ↓
NEW RECOMMENDATION
```

This flow should demonstrate the entire problem statement rather than disconnected features.

\---

# 20\. Exact Build Order

The team must follow this order unless an architectural review explicitly changes it.

```text
01. Project Foundation
        ↓
02. Authentication + RBAC
        ↓
03. Database Foundation
        ↓
04. Competency Framework
        ↓
05. Role-to-Competency Mapping
        ↓
06. Official Profile
        ↓
07. Competency Assessment
        ↓
08. Competency Engine
        ↓
09. Skill Gap Engine
        ↓
10. iGOT Abstraction
        ↓
11. iGOT Mock Adapter
        ↓
12. Course-to-Competency Mapping
        ↓
13. Recommendation Engine
        ↓
14. Personalized Learning Path
        ↓
15. Document Processing
        ↓
16. Embeddings + pgvector
        ↓
17. AI MCQ Generation
        ↓
18. Question Validation
        ↓
19. Quiz Engine
        ↓
20. Quiz Analytics
        ↓
21. Competency Update Engine
        ↓
22. AI Assistant
        ↓
23. Future Skill Intelligence
        ↓
24. Admin Dashboard
        ↓
25. Security Hardening
        ↓
26. Testing
        ↓
27. Deployment
        ↓
28. SIH Demo + Presentation
```

\---

# 21\. Three Major Product Milestones

## Milestone A — Competency Intelligence

```text
Profile
   ↓
Role
   ↓
Required Competencies
   ↓
Assessment
   ↓
Current Competencies
   ↓
Skill Gaps
```

\---

## Milestone B — Personalized Capacity Building

```text
Skill Gap
   ↓
iGOT Courses
   ↓
Course-Competency Mapping
   ↓
Recommendation
   ↓
Learning Path
```

\---

## Milestone C — AI Learning Feedback Loop

```text
Upload Material
   ↓
Extract
   ↓
Generate MCQs
   ↓
Validate
   ↓
Quiz
   ↓
Analyze Performance
   ↓
Update Competency
   ↓
Recalculate Gap
   ↓
Recommend Again
```

\---

# 22\. Architectural Quality Gates

A phase is not considered complete merely because the code runs.

Each phase must pass four gates:

```text
ARCHITECTURE
     ↓
IMPLEMENTATION
     ↓
TESTING
     ↓
REVIEW
```

Before moving to the next phase, verify:

### Functional

```text
Feature works
```

### Technical

```text
Code follows architecture
```

### Security

```text
No obvious authorization/data vulnerabilities
```

### Integration

```text
Existing modules remain functional
```

\---

# 23\. Lead Architect Development Protocol

For every development phase, the implementation process should be:

```text
1. Architecture specification
        ↓
2. AI coding prompt
        ↓
3. Implementation
        ↓
4. Local testing
        ↓
5. Error reporting
        ↓
6. Debugging
        ↓
7. Architecture review
        ↓
8. Acceptance
        ↓
9. Git commit
        ↓
10. Next phase
```

AI coding assistants should **not** be instructed to rewrite the entire project.

They should receive only the current milestone specification.

\---

# 24\. AI Coding Assistant Rules

Claude/Gemini/other coding assistants should follow these rules:

```text
1. Do not rewrite unrelated modules.
2. Do not modify architecture without approval.
3. Do not invent external APIs.
4. Do not hard-code API credentials.
5. Do not expose secrets.
6. Do not bypass RBAC.
7. Do not directly couple application logic to iGOT.
8. Do not blindly trust LLM output.
9. Validate structured AI outputs.
10. Preserve existing functionality.
11. Write tests for new functionality.
12. Explain architectural changes before making major changes.
```

\---

# 25\. Definition of Done

A feature is considered complete only when:

```text
✓ Implementation complete
✓ API tested
✓ Database behavior verified
✓ Frontend behavior verified
✓ Error cases handled
✓ Authorization verified
✓ Relevant unit tests written
✓ Integration tests written where required
✓ No existing functionality broken
✓ Documentation updated
✓ Git commit created
```

\---

# 26\. Final Architecture Summary

The platform is fundamentally a **competency intelligence and personalized capacity-building system**.

The core architecture is:

```text
             ┌──────────────────────┐
             │   OFFICIAL PROFILE   │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │     ASSESSMENT       │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ COMPETENCY PROFILE   │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │     SKILL GAPS       │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │   iGOT COURSE DATA   │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │  RECOMMENDATION      │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │  LEARNING PATH       │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ LEARNING / QUIZ      │
             └──────────┬───────────┘
                        ↓
             ┌──────────────────────┐
             │ COMPETENCY UPDATE    │
             └──────────┬───────────┘
                        ↓
                  RECOMMEND AGAIN
```

Parallel AI learning loop:

```text
UPLOAD
  ↓
EXTRACT
  ↓
EMBED
  ↓
RETRIEVE
  ↓
GENERATE MCQs
  ↓
VALIDATE
  ↓
QUIZ
  ↓
ANALYZE
  ↓
UPDATE COMPETENCY
  ↓
RECOMMEND
```

The architecture therefore creates a continuous **competency feedback system**, rather than a conventional LMS.

\---

# 27\. Immediate Next Step

Do **not** start implementing all modules.

The next development task is:

```text
PHASE 0
PROJECT FOUNDATION
```

The team should first establish:

```text
Next.js
    ↕
FastAPI
    ↕
PostgreSQL
```

along with:

```text
Environment configuration
Repository structure
API versioning
Database connection
Basic health checks
Development documentation
```

Only after Phase 0 passes its acceptance criteria should Phase 1 begin.

