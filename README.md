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
---## 💻 **5. Technical Implementation Details**### **5.1 Database Initialization (Supabase/PostgreSQL)***These scripts establish the relational backbone for the Intelligent Tutoring System.*```sql

-- Enable UUID extension for secure, non-guessable IDs

CREATE EXTENSION IF NOT EXISTS "uuid-ossp";



-- 1. USERS & ROLES

-- Stores student and teacher profiles securely

CREATE TABLE users (

    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

    email TEXT UNIQUE NOT NULL,

    full_name TEXT NOT NULL,

    role TEXT CHECK (role IN ('student', 'teacher', 'admin')),

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()

);



-- 2. SYLLABUS CONCEPTS (The Knowledge Graph)

-- Granular breakdown of subjects (e.g., Subject: "OS", Tag: "Paging")

CREATE TABLE concept_tags (

    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

    subject_code TEXT NOT NULL, -- e.g., "TCS-601"

    tag_name TEXT NOT NULL,     -- e.g., "Virtual Memory"

    weightage INT DEFAULT 1     -- Importance factor (1-5)

);



-- 3. ACADEMIC HEALTH CARD (The "Weakness Radar")

-- Tracks how well a student knows a specific concept

CREATE TABLE mastery_tracker (

    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

    student_id UUID REFERENCES users(id) ON DELETE CASCADE,

    concept_id UUID REFERENCES concept_tags(id),

    mastery_score FLOAT DEFAULT 0.0, -- 0.0 to 100.0

    last_updated TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    UNIQUE(student_id, concept_id)

);



-- 4. REMEDIATION PLANS (AI Output)

-- Stores the personalized study guides generated by Gemini/Groq

CREATE TABLE study_plans (

    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),

    student_id UUID REFERENCES users(id),

    weak_concept TEXT NOT NULL,

    ai_generated_content TEXT NOT NULL, -- The Markdown study guide

    is_completed BOOLEAN DEFAULT FALSE,

    created_at TIMESTAMP DEFAULT NOW()

);

5.2 Backend Logic (FastAPI + AI Service)

The logic that detects weakness and triggers the AI intervention.

File: services/ai_tutor.py

Python



from fastapi import APIRouter, HTTPExceptionfrom pydantic import BaseModelimport random# Initialize Router

router = APIRouter()class StudyRequest(BaseModel):

    student_id: str

    weak_topic: str

    current_score: float@router.post("/generate-rescue-plan")async def generate_exam_rescue_plan(request: StudyRequest):

    """

    Triggers the AI to generate a targeted study plan for a weak topic.

    """

    

    # 1. Construct the Context-Aware Prompt

    system_prompt = f"""

    ROLE: Expert CSE Professor at a top technical university.

    STUDENT CONTEXT: This student has a mastery score of {request.current_score}% 

    in the topic '{request.weak_topic}'. They are at risk of failing this section.

    

    TASK:

    1. Explain '{request.weak_topic}' simply using a real-world analogy.

    2. Provide 3 critical exam-style questions often asked in Vivas.

    3. Write a small 'Trap Code' snippet (C++/Python) that contains a bug related 

       to this concept, and ask the student to find it.

    

    FORMAT: Returns strictly formatted Markdown.

    """



    # 2. Call the AI Model (Pseudocode for Gemini/Groq Integration)

    # response = await ai_client.chat.completions.create(

    #     model="llama-3-70b",

    #     messages=[{"role": "system", "content": system_prompt}]

    # )

    

    # Mock Response for Demonstration

    mock_ai_response = f"""

    ### 🛡️ Rescue Plan: {request.weak_topic}

    

    **1. The Concept:**

    Imagine '{request.weak_topic}' is like a library index card system...

    

    **2. Exam Drill:**

    * Q1: What happens if... ?

    * Q2: Calculate the complexity of...

    

    **3. Debug Challenge:**

    ```cpp

    // Find the memory leak here...

    ```

    """

    

    return {

        "status": "success",

        "plan": mock_ai_response,

        "estimated_time": "15 mins"

    }

5.3 Frontend Dashboard (Next.js + Tailwind)

The visual component that shows students their academic health at a glance.

Component: WeaknessRadar.tsx

TypeScript



import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";

import { Badge } from "@/components/ui/badge";



// Sample Data from API

const topics = [

  { name: "Graph Algorithms", score: 85, status: "safe" },

  { name: "Dynamic Programming", score: 42, status: "critical" },

  { name: "OS Scheduling", score: 65, status: "warning" },

];



export default function WeaknessRadar() {

  return (

    <div className="space-y-4">

      <h2 className="text-2xl font-bold text-slate-800">📊 Academic Health</h2>

      

      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">

        {topics.map((topic) => (

          <Card key={topic.name} className={`border-l-4 ${

            topic.status === 'critical' ? 'border-red-500' : 

            topic.status === 'warning' ? 'border-yellow-500' : 'border-green-500'

          }`}>

            <CardHeader className="pb-2">

              <div className="flex justify-between items-center">

                <CardTitle className="text-sm font-medium text-slate-500">

                  {topic.name}

                </CardTitle>

                {topic.status === 'critical' && (

                  <Badge variant="destructive" className="animate-pulse">

                    Action Needed

                  </Badge>

                )}

              </div>

            </CardHeader>

            <CardContent>

              <div className="text-2xl font-bold">{topic.score}%</div>

              <p className="text-xs text-slate-400 mt-1">

                {topic.status === 'critical' 

                  ? "📉 Falling behind class average" 

                  : "🚀 On track for an A grade"}

              </p>

              

              {topic.status === 'critical' && (

                <button className="mt-3 w-full bg-red-600 hover:bg-red-700 text-white text-sm py-2 rounded-md font-medium transition-colors">

                  🚑 Start Exam Rescue

                </button>

              )}

            </CardContent>

          </Card>

        ))}

      </div>

    </div>

  );

}

🏁 6. Conclusion & Impact

By shifting the focus from "Assignment Submission" to "Concept Mastery," this platform solves the biggest problem in engineering education: Unnoticed Learning Gaps.

This system ensures no student reaches the exam hall unaware of their blind spots, making it a true Academic Companion rather than just a digital logbook.
