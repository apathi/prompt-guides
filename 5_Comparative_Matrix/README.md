# 6. Comparative Matrix (Side‑by‑Side)

▶ Comparison of all Prompt guides from Anthropic x OpenAI x Gemini

| Topic | Anthropic (Claude) | OpenAI (GPT‑5 & Help) | Gemini (Gemini API) |
|-------|-----------|---------|---------|
| Clarity & ordering | "Be clear, direct, detailed"; numbered steps. [A3] | Instructions at top; delimiters. [O1] | "Clear and specific instructions"; constraints. [G1] |
| Examples | Multishot (few‑shot) page. [A4] | Zero‑shot → few‑shot → fine‑tune. [O1] | Few‑shot; positive patterns; consistency. [G1] |
| Reasoning | "Let Claude think (CoT)." [A4] | `reasoning_effort`, planning, agentic flows. [O2] | Patterns + response prefixes (implicit reasoning). [G1] |
| Structure | **XML tags**, nesting. [A2] | Delimiters; examples; agentic sections. [O1][O2][O3] | Response formatting & prefixes. [G1] |
| Tools/Agents | `tool_use` ↔︎ `tool_result`; parallel calls; server vs client tools. [A12] | **Tool preambles**; calibrate eagerness. [O2] | Function calling + prompting strategies; **safety** emphasis. [G1][G2] |
| Long context | Dedicated long‑context tips. [A7] | Persist reasoning via Responses API. [O2] | Long‑context capability; structure with examples. [G1] |
| Safety | Reduce hallucinations; increase consistency; reduce prompt leak. [A10][A11][A13] | Correctness via constraints/examples; parameters. [O1] | Adjustable safety filters. [G2] |
| System prompts | Durable role/tone & guardrails; bookend rules. [A7] | System vs user roles; delimiters. [O1][O3] | System-like role setup; consistent style. [G1] |
| Safety controls granularity | Guardrail patterns; JSON mode. [A10][A11] | Prompt constraints; parameters. [O1] | Tunable category thresholds. [G2] |
| Prompt leak mitigation | Explicit leak‑reduction tactics. [A13] | Avoid exposing hidden inputs. [O1][O3] | Safety guidance; avoid revealing internals. [G1] |
| Reflective/self‑critique | CoT + multi‑pass reasoning. [A4] | Critique/verification passes in agents. [O2] | Encourage examples & concise validation. [G1] |