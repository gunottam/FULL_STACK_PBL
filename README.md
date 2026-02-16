---

# 🎓 Smart Academic Companion

## Intelligent Tutoring System (ITS)

### Department-Level Pilot – Computer Science & Engineering

---

# 1️⃣ Executive Overview

## 🎯 Vision

To transform the traditional Learning Management System (LMS) from a passive submission tracker into an **active academic intelligence platform**.

Instead of asking:

> “Did the student submit?”

We ask:

> “Does the student understand?”

---

## 🏫 Scope

This pilot implementation is restricted to:

* Computer Science & Engineering Department
* Authenticated users with `@geu.ac.in` email
* Department-level usage before full college rollout

This controlled scope ensures:

* Manageable development
* Secure authentication
* High-quality feature depth
* Real institutional relevance

---

# 2️⃣ System Objectives

1. Centralize academic communication
2. Replace WhatsApp-based academic coordination
3. Track student academic engagement
4. Provide AI-powered academic assistance
5. Enable performance analytics for teachers

---

# 3️⃣ Technology Stack

| Layer    | Technology                  | Reason                          |
| -------- | --------------------------- | ------------------------------- |
| Frontend | Next.js 14 (App Router)     | Scalable routing + SSR          |
| UI       | Tailwind CSS + ShadCN UI    | Clean, professional UI          |
| Backend  | FastAPI (Python)            | High-performance async API      |
| Database | Supabase (PostgreSQL)       | Relational integrity + Realtime |
| AI       | Groq API (Llama 3) / Gemini | Fast inference                  |
| Auth     | Google OAuth                | Domain-restricted login         |

---

# 4️⃣ System Architecture

```
User → Next.js Frontend → FastAPI Backend → PostgreSQL Database
                                   ↓
                                AI API
                                   ↓
                             File Storage
```

---

# 5️⃣ Core Modules

---

# 🔐 Module 1: Authentication & User Management (The Gatekeeper)

## Objective

Restrict access strictly to GEU CSE members.

## Features

### 1. Google OAuth Login

* Only `*@geu.ac.in` emails allowed
* Unauthorized domain → access denied

### 2. Role-Based Access Control (RBAC)

Roles:

* Student
* Teacher
* Admin

Permission matrix enforced at backend level.

### Why It Matters

Prevents misuse and ensures academic integrity.

---

# 🏫 Module 2: Academic Core (CRUD Infrastructure)

## Objective

Replace fragmented communication systems.

---

## 📢 Smart Notice Board (Realtime)

### Features:

* Teachers post notices
* Tags: `[Exam]`, `[Placement]`, `[General]`
* Real-time updates via Supabase listeners
* Read receipts
* Filter by tag

### Impact:

Eliminates WhatsApp confusion.

---

## 📝 Assignment Hub

### Teacher:

* Create assignment
* Set deadline
* Upload resource PDF

### Student:

* Submit PDF only
* Auto-lock submission after deadline

### Additional Controls:

* File size validation
* Status: `ontime` or `late`

---

# 💬 Module 3: Doubt Resolution Forum

## Objective

Centralized knowledge repository.

### Features:

* Subject tagging (OS, CN, DBMS, etc.)
* Markdown support for code blocks
* Upvote system ("Found Helpful")
* Mark thread as “Solved”

### Benefit:

Reduces repeated doubt answering.

---

# 🤖 Module 4: AI Companion Layer

## Objective

Practical AI integration without heavy model training.

---

## 1️⃣ AI Doubt Assistant

* Button: “Generate AI Answer”
* AI responds as:

  > Strict Computer Science Professor
* AI reply tagged as: 🤖 AI Suggested

---

## 2️⃣ Quiz Generator

* Teacher uploads lecture text
* AI generates:

  * 5 MCQs
  * 1 short-answer question

---

## 3️⃣ Complexity Analyzer

* Student pastes C++ / Python code
* AI returns:

  * Time complexity (Big-O)
  * Suggested optimization

---

# 📊 Module 5: Academic Analytics Engine

This is what separates your project from a simple web app.

---

## Student Dashboard

* Assignment punctuality rate
* Participation score
* AI usage frequency
* Performance trend chart

---

## Teacher Dashboard

* Submission compliance %
* Most confusing topics (based on tags)
* Class participation heatmap

---

# 🧩 Module 6: Role-Based Access Control Engine

Backend enforced.

### Permission Matrix

| Action            | Student  | Teacher | Admin      |
| ----------------- | -------- | ------- | ---------- |
| Post Notice       | ❌        | ✅       | ✅          |
| Create Assignment | ❌        | ✅       | ✅          |
| Submit Work       | ✅        | ❌       | ❌          |
| Use AI            | ✅        | ✅       | Optional   |
| View Analytics    | Personal | Class   | Department |

Implemented using:

* JWT verification
* Dependency-based role guards in FastAPI

---

# 📁 Module 7: File Management & Security Layer

* MIME type validation
* Max file size limit (5MB)
* Unique hashed filenames
* Secure storage bucket
* Deadline-based locking

Prevents:

* Malicious uploads
* Data overwrite
* Security abuse

---
# 🧠 Module 8: Concept Mastery & Weakness Detection Engine

## (The Core Intelligence Layer)

---

## 🎯 Objective

To automatically detect weak topics for each student based on academic activity and generate personalized exam preparation plans.

This transforms the system from:

> A content management tool
> into
> An intelligent tutoring system.

---

# 📊 How It Works

The system tracks performance at the **concept level**, not just assignment level.

Instead of storing:

* “Scored 14/20 in Assignment 3”

It stores:

* 60% mastery in `#Recursion`
* 85% mastery in `#LinkedList`
* 40% mastery in `#DynamicProgramming`

---

# 🏗️ Step 1: Concept Tagging System

Every academic activity is linked to concept tags.

### Example:

Assignment: "Binary Trees Implementation"

Linked Tags:

* `#Trees`
* `#Recursion`
* `#Pointers`

Quiz Question:
"What is the time complexity of DFS?"

Tagged as:

* `#Graph`
* `#TimeComplexity`

---

# 📈 Step 2: Mastery Score Calculation

For each student, the system calculates:

```
Mastery Score = 
(Weighted Average of Scores related to that Concept)
```

Factors considered:

* Assignment performance
* Quiz results
* Participation in doubt forum
* AI usage correctness (optional enhancement)

---

### Mastery Status Levels

| Score     | Status      | Meaning                      |
| --------- | ----------- | ---------------------------- |
| 80%+      | 🟢 Strong   | No intervention needed       |
| 50–79%    | 🟡 At Risk  | Needs revision               |
| Below 50% | 🔴 Critical | Immediate attention required |

---

# 📊 Student Dashboard View

Each student sees:

### 📌 Academic Health Card

```
Recursion: 72%  → At Risk
Dynamic Programming: 38% → Critical
Operating Systems Scheduling: 84% → Strong
```

Clear. Visual. Actionable.

---

# 🚑 Step 3: AI Exam Rescue Mode

When exam approaches, student clicks:

## “Prepare for Mid-Term”

System logic:

1. Identify topics with mastery < 60%
2. Ignore strong topics
3. Generate targeted study material only for weak areas

---

## 🤖 AI Generates:

For each weak topic:

### 1️⃣ Simplified Concept Explanation

Using real-world analogy

### 2️⃣ 3–5 Exam-Level Questions

Mix of:

* MCQs
* Viva-style
* Short answer

### 3️⃣ Debugging Challenge

Code snippet containing a mistake related to that concept

### 4️⃣ Estimated Study Time

Example:

> “Estimated revision time: 40 minutes”

---

# 🗄️ Database Extension for This Module

Add the following tables:

---

## Concept Tags Table

* id (UUID)
* subject_code
* tag_name
* weightage

---

## Student Concept Mastery Table

* id (UUID)
* student_id (FK → Users)
* concept_id (FK → ConceptTags)
* mastery_score (0–100)
* last_updated

Unique constraint:
(student_id, concept_id)

---

## Study Plans Table

* id (UUID)
* student_id
* weak_concept
* ai_generated_content
* is_completed
* created_at

---

# 📈 Teacher Dashboard Insight

Teachers can view:

* Most weak topics across class
* % of students critical in each concept
* Topic confusion heatmap

Example:

```
Dynamic Programming:
→ 42% students below 50%
```

This allows data-driven teaching intervention.

---

# 🔥 Why This Feature Is Powerful

1. Moves system from reactive to proactive
2. Detects learning gaps before exams
3. Makes AI practical, not decorative
4. Adds measurable academic value
5. Differentiates project from Google Classroom

---

# 🧩 How It Makes Your Project Stand Out in Viva

If professor asks:

“How is this different from LMS?”

Your answer:

> Traditional LMS tracks submissions.
> Our system tracks conceptual mastery and provides AI-based targeted remediation before exams.

That line alone is strong.

---

# 🗄️ Database Design (Core Tables)

## Users

* id (UUID)
* email
* full_name
* role

## Courses

* id
* code
* name
* teacher_id

## Notices

* id
* title
* content
* tags
* author_id
* created_at

## Assignments

* id
* course_id
* title
* due_date

## Submissions

* id
* assignment_id
* student_id
* file_url
* submitted_at
* status

---

# 🚀 Development Roadmap

## Sprint 1

* Setup Next.js + Supabase
* Implement Google OAuth
* Create dashboard shell

## Sprint 2

* Notices + Assignments
* Role-based restrictions

## Sprint 3

* Doubt Forum
* File upload validation

## Sprint 4

* AI integration
* Analytics dashboards

---

# 📈 Scalability Plan

Future expansion:

* Mobile app version
* Attendance tracking
* Concept mastery scoring
* Cross-department rollout
* Integration with official ERP

---

# 🏁 Conclusion

This system:

* Replaces fragmented academic communication
* Enhances academic transparency
* Provides AI-assisted learning
* Enables data-driven teaching decisions
* Maintains strict institutional access control

It is not just a submission portal.

It is a **Department-Level Intelligent Academic Infrastructure**.

---


