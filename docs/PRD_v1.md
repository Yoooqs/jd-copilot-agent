# JD Copilot PRD v1

## 1. Problem Statement
AI PM candidates cannot quickly map JD requirements to their own skills. Existing tools are either generic chatbots (hallucination-prone) or static documents.

## 2. Target User & Persona
- **Primary User:** AI PM job seekers targeting top-tier companies (DeepSeek, Kimi, GMT).
- **User Need:** Fast gap analysis, actionable learning plan, and resume rewriting.

## 3. User Stories (Priority: P0)
1. **As a candidate**, I want to analyze a JD and extract key capabilities so that I know what the company values.
2. **As a candidate**, I want to compare the JD with my resume and identify gaps so that I know where I stand.
3. **As a candidate**, I want to generate a 2-week learning plan so that I can upskill efficiently.
4. **As a candidate**, I want to rewrite my resume bullets so that my resume passes ATS and HR screening.

## 4. Functional Requirements
- **Input:** JD text (from CSV), Resume text (Markdown).
- **RAG Engine:** Retrieve top-3 relevant JD chunks for each query. Must cite sources.
- **Tool Use:** 
  - `compute_gap(jd_capabilities, resume_text)` -> returns gap list.
  - `generate_plan(gaps, weeks)` -> returns structured weekly plan.
  - `rewrite_resume_bullet(resume_text, target_capability)` -> returns 3 quantifiable bullets.
- **Output:** Structured JSON + human-readable Markdown.

## 5. Non-Functional Requirements
- **Cost:** < 0.05 RMB per query.
- **Latency:** P95 < 15 seconds.
- **Privacy:** Resume must be desensitized before processing.
- **Reliability:** Graceful fallback if retrieval fails (answer "I don't know" instead of hallucinating).

## 6. Non-goals (Out of Scope)
- No multi-agent orchestration.
- No fine-tuning (rely on RAG for knowledge).
- No mobile app or login system.
- No large-scale crawling (manual collection of 30 JDs).

## 7. Success Metrics (KPIs)
- **Citation Accuracy:** > 80% (Grounding truth).
- **Task Completion Rate:** > 85% (User can complete 3-step workflow).
- **User Satisfaction (Optional):** Manual rating > 4/5.

## 8. Milestones & Release Plan
- **v0.1 (Day 1-2):** Data collection & Eval set built.
- **v0.2 (Day 3-4):** RAG pipeline working, baseline vs RAG comparison.
- **v0.3 (Day 5):** Tool Use integrated (gap, plan, rewrite).
- **v1.0 (Day 6-14):** Agent Loop, Context Engineering, Harness, Demo ready.

## 9. Risks & Mitigation
- **Hallucination:** Mitigated by RAG + Citation Check.
- **Tool Misuse:** Mitigated by Harness (whitelist + max retries).