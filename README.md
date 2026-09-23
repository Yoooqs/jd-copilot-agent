# JD Copilot Agent
> An AI PM candidate copilot that analyzes JDs and diagnoses skill gaps using Agentic RAG.

## 🎯 Problem
AI PM candidates struggle to map real JDs to their own skills. Pure LLMs (Baseline) hallucinate heavily (0% citation accuracy in my tests, generating algorithm engineer requirements for PM roles), causing severe trust issues.

## 💡 Solution & Core Value
Built an Agentic RAG system not as a "perfect toy", but as a **sandbox to explore the boundaries of AI product engineering**. 
I didn't just build it; I stress-tested it, found its limits (hallucinations, JSON deadlocks, over-optimization cheating), and engineered solutions:
- **RAG (Fix Hallucinations)**: Achieved 83% citation accuracy in MVP tests.
- **Tool Use & Agent Loop (Fix Deadlocks)**: Engineered self-correction loops with a max-3-iteration guardrail.
- **Harness Guardrails (Fix Cheating)**: Implemented Domain Isolation to prevent the LLM from "rewriting" B2B SaaS experience into LLM training experience (intercepted 100%).
- **Context Engineering (Cost Optimization)**: Slashed token cost by 68% with <0.5 score quality degradation.
- **Observability Design (&Future Roadmap)**: Exposed LLM's "physical metric blind spot" via single-track logging MVP test (est. 12s vs actual 154s latency). Proposed a dual-track logging architecture (Semantic + Physical) in the PRD as a V2 roadmap for production rollout.

## 🏗️ Architecture & Core Features
- **RAG**: Hybrid Search + Rerank (BAAI/bge-m3).
- **Tool Use**: 3 custom tools (compute_gap, generate_plan, rewrite_resume_bullet).
- **Agent Loop & Harness**: Self-correction loop with Domain Isolation guardrails.
- **Context Engineering**: Compressed resume (300 chars) to reduce token cost.
- **Observability Roadmap**: Exposed LLM's "physical metric blind spot" in MVP testing (est. 12s vs actual 154s latency). Proposed a V2 dual-track logging architecture (Semantic logs by Agent + Physical metrics by system) based on this finding.

## 📊 Key Results (A/B Testing Insights)
- **Citation Accuracy (MVP Test)**: In a 6-case sample, Baseline yielded 0% valid citations (with hallucinations), while RAG achieved 83% valid citations grounded in JD text.
- **Cost Efficiency**: Reduced token consumption by 68% (from 29,372 to 9,376 tokens) in A/B tests, with only a 0.5/10 quality drop.
- **Safety Guardrails**: Successfully intercepted 100% of cross-domain hallucination attempts (e.g., rewriting B2B SaaS experience into LLM training experience).

## 🚀 How to Run
- Platform: Dify Cloud
- Model: DeepSeek-V3 (via SiliconFlow)
- Try it here: [Insert Demo Link]
