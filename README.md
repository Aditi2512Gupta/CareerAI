# CareerAI

## AI Career Guidance and Skill Recommendation System

CareerAI is a web-based intelligent system designed to help students and early-career professionals make informed career decisions. It analyzes academic background, interests, and existing skills to recommend suitable career paths, identify skill gaps, and suggest relevant learning resources.

## Key Features

- User registration and authentication
- Profile creation and management
- Skill and interest assessment
- AI-based career path recommendations
- Skill-gap analysis
- Learning resource recommendations
- Learning progress tracking
- Resume/profile skill import
- Career search and filtering
- Recommendation feedback
- Admin content management
- Notifications and reporting support

## Project Scope

The current project focuses on a responsive web application for students, fresh graduates, and early-career professionals. Direct job placement, corporate ATS/recruitment integration, real-time one-to-one video counselling, native mobile applications, and payment processing are outside the current release scope.

## Requirement Overview

### Functional Requirements

| ID | Requirement |
|---|---|
| FR-01 | User Registration & Authentication |
| FR-02 | Profile Creation |
| FR-03 | Career Path Recommendation |
| FR-04 | Skill Gap Analysis |
| FR-05 | Learning Resource Recommendation |
| FR-06 | Progress Tracking |
| FR-07 | Resume/Profile Skill Import |
| FR-08 | Search & Filter Careers |
| FR-09 | Feedback Submission |
| FR-10 | Admin Content Management |
| FR-11 | Notifications |
| FR-12 | Reporting Dashboard |

### Non-Functional Requirements

- **Performance:** Recommendations should be generated within the specified response time under normal conditions.
- **Usability:** The interface should be intuitive for first-time users.
- **Security:** Sensitive user information should be protected and encrypted.
- **Availability:** The system should remain available during normal use and evaluation.
- **Scalability:** The architecture should support future growth.
- **Compatibility:** The web application should work across major modern browsers and screen sizes.
- **Maintainability:** The system should use modular and documented components.
- **Reliability:** Invalid input and service failures should be handled gracefully.

## System Diagrams

### 1. DFD – Level 0

```mermaid
flowchart LR
    S[Student / Job Seeker] -->|Profile & Assessment Data| P((CareerAI Process))
    P -->|Career Recommendations, Skill Gaps & Resources| S
    A[Administrator / Career Counsellor] -->|Career, Skill & Resource Updates| P
    P -->|Read / Update Reference Data| D[(Career / Skill / Learning Resource Data)]
    D -->|Reference Data| P
```

### 2. UML Use Case Diagram

```mermaid
flowchart LR
    Student[Student / Learner]
    Admin[Administrator]
    Counselor[Career Counsellor]
    Market[Job Market Data Feed]

    subgraph System[AI Career Guidance & Skill Recommendation System]
        A((Take Skill Assessment))
        B((View Career Recommendations))
        C((Enroll in Learning Resource))
        D((Track Learning Progress))
        E((Manage Career Paths))
        F((Configure Recommendation Rules))
        G((View Analytics Dashboard))
        H((Analyze Skill Profile))
        I((Match Career Paths))
    end

    Student --> A
    Student --> B
    Student --> C
    Student --> D
    Counselor --> D
    Admin --> E
    Admin --> F
    Market --> G
    A -.->|include| H
    B -.->|include| I
```

### 3. UML Class Diagram

```mermaid
classDiagram
    class User {
      UUID userId
      String name
      String email
      List~Skill~ skills
      register()
      login()
    }
    class SkillAssessment {
      UUID assessmentId
      UUID userId
      Map responses
      DateTime completedAt
      submit()
    }
    class Skill {
      UUID skillId
      String name
      Enum level
    }
    class CareerPath {
      UUID pathId
      String title
      List~Skill~ requiredSkills
      Decimal avgSalary
    }
    class Feedback {
      UUID feedbackId
      UUID userId
      Int rating
      String comments
    }
    class RecommendationEngine {
      matchSkills()
      rankCareerPaths()
      generateRecommendations()
    }
    class LearningResource {
      UUID resourceId
      String title
      String provider
      String skillTag
      String url
    }

    User "1" --> "1..*" SkillAssessment : takes
    SkillAssessment "*" --> "*" Skill : uses
    CareerPath "1" --> "*" Skill : requires
    User --> Feedback : writes
    Feedback --> RecommendationEngine : rates
    User --> RecommendationEngine : requests
    SkillAssessment --> RecommendationEngine : feeds
    RecommendationEngine --> CareerPath : matches
    RecommendationEngine --> LearningResource : recommends
```

### 4. UML Sequence Diagram

```mermaid
sequenceDiagram
    actor Student
    participant Client as Client App
    participant API as API Gateway
    participant Engine as Recommendation Engine
    participant DB as Career / Skill DB
    participant Notify as Notification Service

    Student->>Client: Submit assessment answers
    Client->>API: POST /assessment
    API->>Engine: Analyze skills
    Engine->>DB: Fetch career paths
    DB-->>Engine: Matching career data
    Engine-->>API: Ranked recommendations
    API-->>Client: Recommendation response
    Client-->>Student: Display results
    Engine->>Notify: Emit recommendation.ready event

    Note over API,Engine: If assessment data is incomplete,
a validation error requests additional responses.
```

### 5. UML Activity Diagram

```mermaid
flowchart TD
    Start([Receive Assessment Submission]) --> V[Validate Responses]
    V --> C[Compute Skill Profile]
    C --> M[Run Matching Algorithm]
    M --> Q{Match Quality?}
    Q -->|Strong| S[Generate Top Recommendations]
    Q -->|Moderate| R[Generate Recommendations + Resources]
    Q -->|Weak| W[Request Additional Assessment]
    S --> End([Deliver Results to Student])
    R --> End
    W --> End
```

### 6. UML State Machine Diagram

```mermaid
stateDiagram-v2
    [*] --> CREATED
    CREATED --> ANALYZING : valid
    ANALYZING --> GENERATED : rank
    ANALYZING --> FAILED : error
    GENERATED --> DELIVERED : deliver
    DELIVERED --> VIEWED : open
    DELIVERED --> DISMISSED : dismiss
    VIEWED --> ACCEPTED : accept
    GENERATED --> EXPIRED : timeout
    DISMISSED --> [*]
    FAILED --> [*]
    EXPIRED --> [*]
    ACCEPTED --> [*]
```

### 7. UML Component Diagram

```mermaid
flowchart LR
    Client[Student / Counselor Client] --> Dashboard[Web Dashboard]
    Dashboard --> API[Recommendation API]
    API --> Engine[Recommendation Engine]
    API --> Notify[Notification Service]
    Engine --> DB[(PostgreSQL)]
    Engine --> Cache[(Redis / Cache)]
    API --> Audit[Audit & Logging]
    Market[Job Market Data Feed] --> API
```

### 8. UML Deployment Diagram

```mermaid
flowchart TB
    Device[Client Device\nBrowser / Mobile App]
    Web[Web Server\nReact / Static Assets]
    App[Application Server\nRecommendation API\nRecommendation Engine\nML Scoring Service]
    DB[(PostgreSQL\nUsers, Skills, Paths)]
    Redis[(Redis\nCache, Sessions)]
    Ext[Simulated External Services\nJob Market API\nNotification Service]

    Device -->|HTTPS| Web
    Web -->|HTTPS| App
    App -->|SQL| DB
    App -->|Redis Protocol| Redis
    App -->|API Calls / Events| Ext
```

## Rule Table / Decision Table

The recommendation logic considers the user's skill level and interest match to decide the next action.

| Rule | Skill Level | Interest Match | System Action | Priority |
|---|---|---|---|---|
| R1 | High | Yes | Recommend strongly matched career path | Must Have |
| R2 | High | No | Suggest alternative career paths based on profile | Should Have |
| R3 | Low/Medium | Yes | Recommend career, identify skill gaps, and suggest learning resources | Must Have |
| R4 | Low/Medium | No | Request/refine assessment information before recommending | Should Have |

## Requirement Prioritization – MoSCoW

- **Must Have:** Registration, profile creation, career recommendation, skill-gap analysis, learning resource recommendation, and admin content management.
- **Should Have:** Progress tracking, resume skill import, and career search/filtering.
- **Could Have:** Feedback and notifications.
- **Won't Have (current release):** Reporting dashboard.

## Stakeholders

- Students / Job Seekers
- Academic Institutions
- Career Counsellors / Mentors
- System Administrators
- Content & Course Providers
- Development Team
- Project Supervisor / Evaluator

## Repository Documentation

This repository contains the Software Engineering project documentation and system models for CareerAI, including the Requirement Report, SRS, UML models, DFD, and Rule Table.

## Academic Project

**Subject:** Software Engineering  
**Project:** AI Career Guidance and Skill Recommendation System  
**Project Name:** CareerAI
