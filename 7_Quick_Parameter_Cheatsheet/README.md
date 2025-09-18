# 8. Quick Parameter Cheatsheet

- **Model:** Prefer the most capable for your task; consider cost/latency trade‑offs. [O1]
- **Temperature:** 0 for factual/consistent outputs; higher for creative. [O1]
- **Max tokens / Stop:** Bound output, enforce structure. [O1]
- **Reasoning effort (OpenAI) : [O2]**
	Low/medium/high to modulate depth and tool‑calling persistence in agentic flows.
	Rule of thumb - Start minimal or low. Escalate only on uncertainty or failure to meet acceptance criteria. Pair with a tool budget and a clear stop condition.[O2]
	- minimal — fastest. Use when answers are extractive or all facts are in provided context. Tool budget often 0. Stop on first valid output.[O2][O3]
	- low — quick checks. One verification step or 1 tool call for confirmation. Useful for simple retrieval or light transformation.[O2]
	- medium — balanced. Plan briefly, then act. 1–2 tool calls. Good default for multi-step but not high-stakes tasks.[O3]
	- high — thorough. Explicit plan, multiple hypotheses, stronger verification. Reserve for correctness-critical tasks; set strict stop conditions and cite sources.[O2][O3]

Classifying by Task type [O2] -
- Fast extractive task
	- temperature=0, reasoning_effort=minimal, tool_budget=0, strict JSON example.
- Balanced verify task
	- temperature=0–0.3, reasoning_effort=medium, tool_budget=1, stop_condition defined.
- High-stakes analysis
	- temperature=0–0.2, reasoning_effort=high, tool_budget=2, acceptance criteria + citations required.