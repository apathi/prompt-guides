# 1. Core Principles

▶ [1.1 Structure Prompts & Outputs](1.1_Structure_Prompts.md)
	- Separate **instructions / input / examples / format**; ask for **structured output** (JSON/XML) and keep examples consistent. [A2][O1][O3][G1]

▶ [1.2 Ground with Reference Context (Reduce Hallucinations)](1.2_Grounding_Context.md)
	- Provide documents/snippets and **constrain** answers to them; encourage "I don't know" if insufficient. [A10][G1]

▶ [1.3 Show Examples (Zero‑Shot → Few‑Shot → Fine‑Tune)](1.3_Examples_Few_Shot.md)
	- Start **zero‑shot**, then add **few‑shot** examples to demonstrate style/format; fine‑tune only if needed. Gemini encourages consistent, positive patterns. [O1][G1]

▶ [1.4 Decompose & Chain Tasks](1.4_Decompose_Tasks.md)
	- Break complex jobs into **steps** (analyze → plan → do → verify) or **chain** prompts with clear handoffs; GPT‑5 notes better performance when separating tasks across agent turns. [A6][O2]

▶ [1.5 Let the Model "Think" (Reasoning)](1.5_Reasoning.md)
	- For reasoning‑heavy tasks, ask for brief step‑by‑step analysis or a plan; GPT‑5 adds `reasoning_effort` to control depth. [A4][O2]

▶ [1.6 Specify Output Style, Length & Schema](1.6_Output_Specifications.md)
	- Define tone, length bounds, and exact schema; Anthropic offers JSON‑mode consistency tips; Gemini shows response formatting patterns. [A11][G1]

▶ [1.7 Iterate, Evaluate, and Version](1.7_Iteration_Evaluation.md)
	- Maintain prompt **versions**, run a fixed eval suite, categorize failures (format/facts/style), and note deltas. [A8]

▶ [1.8 Lightweight Evaluation Framework (Scoring)](1.8_Evaluation_Framework.md)
	- Build a tiny, repeatable eval harness (20–50 cases) with **scoring rubrics** (e.g., format=1, factuality=2, style=1). Track deltas per prompt version. [A8][O1]

▶ [1.9 Use Tools & Retrieval Thoughtfully](1.9_Tools_Retrieval.md)
	- Prefer native **tool/function calling** over parsing. Define tool names, params, and guardrails; be explicit when to use which tool. [A12]

▶ [1.10 Long‑Context Hygiene](1.10_Long_Context_Hygiene.md)
	- Keep instructions at **top** (optionally repeat key rules at end), and segment large inputs clearly (e.g., XML tags). [A7][A2]

▶ [1.11 Safety & Guardrails](1.11_Safety_Guardrails.md)
	- Use safety prompts and platform safety settings; throttle unsafe categories when needed; design refusals and "ask‑before‑acting" rules. [A10][A13][G2]

▶ [1.12 System Prompts (Persistent Role & Guardrails)](1.12_System_Prompts.md)
	- Use **system prompts** to set durable role, tone, citation rules, refusal policy, and tool usage defaults; keep user prompts task-specific. Reinforce key rules at the end ("bookend" pattern). [A7][A3]