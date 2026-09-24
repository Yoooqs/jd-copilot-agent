# JD Copilot Agent Architecture

```mermaid
flowchart TD
    User([User Input]) --> Dify[Dify Agent / System Prompt]
    Dify --> Variables[Global Variable: resume_text]
    Dify --> RAG{Hybrid Search}
    RAG --> |Top K = 5| KB[(JD Knowledge Base)]
    
    Dify --> ToolUse{Tool Use}
    ToolUse --> |compute_gap| Gap[Gap Analysis]
    ToolUse --> |generate_plan| Plan[Learning Plan]
    ToolUse --> |rewrite_resume_bullet| Rewrite[Resume Rewriting]
    
    Gap & Plan & Rewrite --> Loop{Agent Loop: Score >= 8?}
    Loop --> |No| ToolUse
    Loop --> |Yes| Guardrails{Harness Check}
    
    Guardrails --> |Cross-Domain| Reject[Reject: Domain Mismatch]
    Guardrails --> |Compliant| Output([Final Output + Semantic Log])
    
    Output -.-> |V2 Roadmap| Metrics[Physical Logs: Latency/Tokens]