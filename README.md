# 🎓 Smart Academic Companion: Intelligent Tutoring System (ITS)
### *A Next-Generation Academic Operating System for Computer Science Education*

---

## 🚀 **1. Executive Summary**
**Project Goal:** To transform the traditional Learning Management System (LMS) from a passive record-keeper into an **active academic partner**.

**The Core Innovation:** unlike Google Classroom, which only tracks *submission status* (Turned In/Missing), this platform tracks **Conceptual Mastery**. It uses AI to act as a **personal diagnostic lab**, identifying a student's weak topics (e.g., "Weak in Pointers") and providing targeted remediation before exams.

**Target Audience:** Computer Science & Engineering (CSE) Department.

---

## 🏗️ **2. System Architecture**

### **The Tech Stack**
We have selected a modern, scalable, and industry-standard stack to ensure performance and maintainability.

| Layer | Technology | Justification |
| :--- | :--- | :--- |
| **Frontend** | **Next.js 14** (React) | Server-side rendering for fast load times; strictly typed for reliability. |
| **Styling** | **Tailwind CSS + ShadCN** | Clean, accessible, and professional UI components. |
| **Backend** | **FastAPI** (Python) | High-performance async API; native support for AI/ML libraries. |
| **Database** | **Supabase** (PostgreSQL) | Relational integrity for academic data; built-in Realtime capabilities. |
| **AI Engine** | **Groq / Gemini API** | Ultra-low latency inference for instant student feedback. |
| **Auth** | **Google OAuth** | Restricted strictly to `*.geu.ac.in` domains for security. |

---

## 🧠 **3. Core Modules & Features**

### **Module A: The Knowledge Graph (Syllabus Mapper)**
*The foundation of the system. It maps the curriculum to trackable data points.*

* **Syllabus Digitization:** Teachers upload syllabus units (e.g., "Data Structures").
* **Auto-Tagging:** The system breaks units into granular **Concept Tags** (e.g., `#Recursion`, `#LinkedLists`, `#TimeComplexity`).
* **Value:** Every assignment, quiz, and doubt is linked to these tags, enabling precise tracking.

### **Module B: The "Weakness Radar" (Flagship Feature)**
*The diagnostic engine that creates a personalized "Academic Health Card" for every student.*

* **Real-Time Analytics:**
    * *Input:* Assignment scores, Quiz performance, Doubt history.
    * *Process:* Calculates a **Mastery Score (0-100%)** for each Concept Tag.
* **Visual Dashboard:**
    * **🟢 Strong Topics:** > 80% Mastery (No action needed).
    * **🟡 At Risk:** 50-79% Mastery (Warning).
    * **🔴 Critical Weakness:** < 50% Mastery (Immediate Alert).
* **The "Gap Analysis":**
    > *"Student X has an A in generic programming but is failing in Memory Management."*

### **Module C: Exam Rescue Mode (AI Intervention)**
*The remediation loop. It doesn't just show the problem; it fixes it.*

* **Targeted Study Plans:** When a student clicks *"Prepare for Mid-Terms"*, the AI ignores strong topics and focuses **only** on the Red Zones.
* **Adaptive Content Generation:**
    1.  **Concept Recap:** AI generates a simplified summary of the weak topic (e.g., "Pointers explained with a real-world analogy").
    2.  **Drill Questions:** Generates 3-5 distinct questions targeting the specific misconception.
    3.  **Code Analysis:** For CSE, it asks students to debug a snippet related to the weak topic.

### **Module D: Academic Operations (The "Manager")**
*Essential tools to replace fragmented communication channels (WhatsApp).*

* **Smart Notice Board:** Real-time broadcasts with "Read Receipts" and priority levels (Urgent/Normal).
* **Doubt Resolution Forum:** A StackOverflow-style Q&A board where answers are visible to the whole class, reducing repetitive queries for teachers.
* **Assignment Vault:** Centralized submission portal with strict deadline enforcement and plagiarism pre-checks.

---

## 🗄️ **4. Database Schema Design**
*A relational structure designed to link students to concepts.*

```sql
-- 1. TRACKING MASTERY (The Brain) --
TABLE Student_Concept_Mastery {
  id: UUID (PK)
  student_id: UUID (FK -> Users)
  concept_tag_id: UUID (FK -> Syllabus)
  mastery_score: FLOAT (0.0 - 100.0)
  last_assessed: TIMESTAMP
}

-- 2. REMEDIATION (The Cure) --
TABLE Study_Sessions {
  id: UUID (PK)
  student_id: UUID (FK -> Users)
  target_concept: STRING
  ai_generated_plan: TEXT
  completion_status: BOOLEAN
}

-- 3. ACADEMIC DATA (The Input) --
TABLE Assignments {
  id: UUID (PK)
  title: STRING
  related_concept_tags: ARRAY[STRING] -- Links work to concepts!
  due_date: TIMESTAMP
}
