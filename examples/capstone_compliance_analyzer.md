# CAPSTONE Example: Compliance Analyzer

## Overview

This example demonstrates all best practices from the guide in a real-world compliance analysis scenario. It combines:

- **System prompt** (persistent role & rules)
- **Tool preamble** (OpenAI-style: plan → act → narrate → stop)
- **Safety budget** (explicit limits on tool calls and unsafe inputs)
- **Instructions first, reminders last** (bookended constraints)
- **XML segmentation** (`<instructions>`, `<examples>`, `<context>`, `<format>`)
- **Few-shot example** (short, consistent, schema-aligned)
- **Schema + format template** (guarantees structured output)
- **Guardrails** ("don't invent KPIs," "safe refusal," JSON-only output)
- **Long-context strategy** (summary cap, repetition of key rules)
- **Self-containment** (evaluation criteria embedded)

## Implementation

### OpenAI GPT-5 Style

```typescript
// System prompt
{
  "role": "system",
  "content": `<role>You are a compliance analyst reviewing corporate policies.</role>

<instructions>
- From <context/>, extract Key Performance Indicators (KPIs) and flag anomalies
- Output <summary> (≤120 words) and <table> with rows: {kpi, value, trend, note}
- Also produce <findings> with rule_id, excerpt, severity (low/medium/high)
- Never invent KPIs not explicitly mentioned in context
- If uncertain about a value, mark it as "unclear" rather than guessing
- Refuse to analyze personal data or confidential internal communications
</instructions>

<tool_preambles>
- Restate user goal: "Analyze compliance metrics from provided policy documents"
- Plan:
  1) Read the <context/> carefully for explicit KPIs and targets
  2) Use search(q) only if external verification is needed for regulatory standards
  3) Use calculator(expr) for percentage calculations or target comparisons
- While executing: explain each step briefly
- Stop after ≤2 tool calls OR once all KPIs are identified and analyzed
- If still uncertain after tool budget: say "I couldn't confirm" and list what's missing
</tool_preambles>

<format>
<summary>...</summary>
<table>
  <row><kpi>Training completion</kpi><value>67%</value><trend>below target</trend><note>Gap of 23%</note></row>
</table>
<findings>
  <item><rule_id>Sec3.2</rule_id><excerpt>Training rate 67% vs target 90%</excerpt><severity>high</severity></item>
</findings>
</format>`
}

// Parameters
{
  "reasoning_effort": "medium",  // Balanced planning and execution
  "temperature": 0.1,            // Low for factual consistency
  "max_tokens": 1000,           // Bounded output
  "tools": [
    {
      "name": "search",
      "description": "Search for regulatory standards or compliance benchmarks",
      "input_schema": {
        "type": "object",
        "properties": {
          "query": {"type": "string"}
        },
        "required": ["query"]
      }
    },
    {
      "name": "calculator",
      "description": "Calculate percentages or compare values",
      "input_schema": {
        "type": "object",
        "properties": {
          "expression": {"type": "string"}
        },
        "required": ["expression"]
      }
    }
  ]
}

// User prompt with context
{
  "role": "user",
  "content": `<context>
Policy Section 3.2:
- Employee training completion rate: 67% (target 90%)
- Incident response drill: last held 24 months ago (target: annually)
- Vendor compliance checks: 95% (target 95%)

Policy Section 5.1:
- Customer data breach response time: 120 hours (SLA: 72 hours)
- Data retention compliance: 89% (target 95%)
</context>

Analyze these compliance metrics and provide findings with severity levels.

REMINDER: Only extract KPIs explicitly mentioned. Use ≤2 tool calls. Output must follow exact XML format above.`
}
```

### Anthropic Claude Style

```typescript
// System message with tool specifications
{
  "role": "user",
  "content": `<role>You are a compliance analyst reviewing corporate policies.</role>

<instructions>
- From <context/>, extract KPIs and anomalies
- Output <summary> (≤120 words) and <table> rows {{kpi, value, trend, note}}
- Also produce <findings> with rule_id, excerpt, severity (low/medium/high)
- Never fabricate metrics not in source material
- If data is incomplete, state limitations explicitly
</instructions>

<context>
Policy Section 3.2:
- Employee training completion rate: 67% (target 90%)
- Incident response drill: last held 24 months ago (target: annually)
- Vendor compliance checks: 95% (target 95%)

Policy Section 5.1:
- Customer data breach response time: 120 hours (SLA: 72 hours)
- Data retention compliance: 89% (target 95%)
</context>

<format>
<summary>...</summary>
<table>
  <row><kpi>Training completion</kpi><value>67%</value><trend>below target</trend><note>Gap of 23%</note></row>
  <row><kpi>Incident drill</kpi><value>24 months</value><trend>stale</trend><note>Should be annual</note></row>
  <row><kpi>Vendor checks</kpi><value>95%</value><trend>on target</trend><note>Meets SLA</note></row>
  <row><kpi>Breach response</kpi><value>120 hours</value><trend>over SLA</trend><note>Exceeds 72h limit</note></row>
  <row><kpi>Data retention</kpi><value>89%</value><trend>below target</trend><note>6% gap</note></row>
</table>
<findings>
  <item><rule_id>Sec3.2</rule_id><excerpt>Training rate 67% vs target 90%</excerpt><severity>high</severity></item>
  <item><rule_id>Sec3.2</rule_id><excerpt>Incident drill 24 months vs annual target</excerpt><severity>medium</severity></item>
  <item><rule_id>Sec5.1</rule_id><excerpt>Breach response time 120h vs SLA 72h</excerpt><severity>critical</severity></item>
  <item><rule_id>Sec5.1</rule_id><excerpt>Data retention 89% vs target 95%</excerpt><severity>medium</severity></item>
</findings>
</format>

REMINDER: Extract only explicit KPIs. Follow XML format exactly. Mark uncertain values clearly.`
}

// Tool specifications for Claude API
[
  {
    "name": "search_regulations",
    "description": "Search for regulatory compliance standards",
    "input_schema": {
      "type": "object",
      "properties": {
        "query": {"type": "string"}
      },
      "required": ["query"]
    }
  },
  {
    "name": "calculate",
    "description": "Perform calculations for compliance metrics",
    "input_schema": {
      "type": "object",
      "properties": {
        "expression": {"type": "string"}
      },
      "required": ["expression"]
    }
  }
]
```

### Google Gemini Style

```typescript
// Gemini API configuration with safety settings
{
  "model": "gemini-2.0-flash",
  "safety_settings": [
    {
      "category": "HARM_CATEGORY_HARASSMENT",
      "threshold": "BLOCK_MEDIUM_AND_ABOVE"
    },
    {
      "category": "HARM_CATEGORY_HATE_SPEECH",
      "threshold": "BLOCK_MEDIUM_AND_ABOVE"
    },
    {
      "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
      "threshold": "BLOCK_MEDIUM_AND_ABOVE"
    },
    {
      "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
      "threshold": "BLOCK_MEDIUM_AND_ABOVE"
    }
  ],
  "generation_config": {
    "temperature": 0.1,           // Low for factual consistency
    "max_output_tokens": 1000,    // Bounded output
    "top_p": 0.8,                // Focused sampling
    "top_k": 40                   // Limited vocabulary selection
  },
  "tools": [
    {
      "function_declarations": [
        {
          "name": "search_compliance_standards",
          "description": "Search for regulatory compliance standards and benchmarks",
          "parameters": {
            "type": "object",
            "properties": {
              "query": {
                "type": "string",
                "description": "Search query for compliance information"
              }
            },
            "required": ["query"]
          }
        },
        {
          "name": "calculate_metrics",
          "description": "Calculate compliance percentages and compare values",
          "parameters": {
            "type": "object",
            "properties": {
              "expression": {
                "type": "string",
                "description": "Mathematical expression to evaluate"
              }
            },
            "required": ["expression"]
          }
        }
      ]
    }
  ],
  "tool_config": {
    "function_calling_config": {
      "mode": "AUTO"  // Let model decide when to use tools
    }
  }
}

// User message with few-shot examples and clear instructions
{
  "role": "user",
  "content": `You are a compliance analyst reviewing corporate policies. Your task is to extract Key Performance Indicators (KPIs) from policy documents and flag compliance anomalies.

INSTRUCTIONS:
- From the provided context, extract only explicitly mentioned KPIs and their targets
- Output a summary (≤120 words), table with KPI analysis, and findings with severity levels
- Never fabricate metrics not present in source material
- If data is incomplete, state limitations explicitly
- Refuse to analyze personal data or confidential internal communications

EXAMPLES OF PROPER ANALYSIS:

Example 1 - Training Compliance:
Context: "Staff certification rate: 82% (target 90%)"
Analysis: Training certification at 82% falls 8 percentage points below target.
Findings: Moderate compliance gap requiring attention.

Example 2 - Security Metrics:
Context: "Incident response time: 48 hours (SLA: 24 hours)"
Analysis: Response time exceeds SLA by 24 hours (100% over target).
Findings: Critical SLA breach requiring immediate remediation.

Example 3 - Vendor Compliance:
Context: "Vendor assessments: 95% complete (target 95%)"
Analysis: Vendor assessment completion meets established target.
Findings: Compliant - no action required.

REQUIRED OUTPUT FORMAT:
<summary>
[≤120 word executive summary of key findings]
</summary>

<table>
  <row><kpi>[KPI name]</kpi><value>[actual value]</value><trend>[above/below/on target]</trend><note>[brief explanation]</note></row>
</table>

<findings>
  <item><rule_id>[section reference]</rule_id><excerpt>[specific excerpt]</excerpt><severity>[low/medium/high/critical]</severity></item>
</findings>

NOW ANALYZE THE FOLLOWING POLICY DOCUMENTS:

CONTEXT:
Policy Section 3.2:
- Employee training completion rate: 67% (target 90%)
- Incident response drill: last held 24 months ago (target: annually)
- Vendor compliance checks: 95% (target 95%)

Policy Section 5.1:
- Customer data breach response time: 120 hours (SLA: 72 hours)
- Data retention compliance: 89% (target 95%)

CONSTRAINTS:
- Use ≤2 function calls if external verification needed
- Extract only KPIs explicitly mentioned in context
- Follow exact XML format shown above
- Mark uncertain values as "unclear" rather than guessing

Analysis:`
}
```

### Expected Output

```xml
<summary>
Analysis reveals significant compliance gaps across training and incident response. Employee training completion at 67% falls 23 percentage points below the 90% target. Incident response drills are severely outdated at 24 months vs annual requirement. Customer data breach response time exceeds SLA by 48 hours. Only vendor compliance checks meet targets at 95%.
</summary>

<table>
  <row><kpi>Training completion</kpi><value>67%</value><trend>below target</trend><note>Gap of 23%</note></row>
  <row><kpi>Incident drill</kpi><value>24 months</value><trend>stale</trend><note>Should be annual</note></row>
  <row><kpi>Vendor checks</kpi><value>95%</value><trend>on target</trend><note>Meets SLA</note></row>
  <row><kpi>Breach response</kpi><value>120 hours</value><trend>over SLA</trend><note>Exceeds 72h limit</note></row>
  <row><kpi>Data retention</kpi><value>89%</value><trend>below target</trend><note>6% gap</note></row>
</table>

<findings>
  <item><rule_id>Sec3.2</rule_id><excerpt>Training rate 67% vs target 90%</excerpt><severity>high</severity></item>
  <item><rule_id>Sec3.2</rule_id><excerpt>Incident drill 24 months vs annual target</excerpt><severity>medium</severity></item>
  <item><rule_id>Sec5.1</rule_id><excerpt>Breach response time 120h vs SLA 72h</excerpt><severity>critical</severity></item>
  <item><rule_id>Sec5.1</rule_id><excerpt>Data retention 89% vs target 95%</excerpt><severity>medium</severity></item>
</findings>
```

## Best Practices Demonstrated

✅ **System prompt**: Fixes persona, tone, refusal policy

✅ **Tool preamble**: Plan → act → narrate → stop; ensures transparent agent behavior

✅ **Safety budget**: Explicit limits on tool calls, unsafe inputs, and refusal triggers

✅ **Instructions first, reminders last**: Bookended constraints to survive long-context drift

✅ **XML segmentation**: `<instructions>`, `<context>`, `<format>` ensure clarity

✅ **Few-shot example**: Short, consistent, schema-aligned illustration

✅ **Schema + format template**: Guarantees structured output that downstream apps can parse

✅ **Guardrails**: "Don't invent KPIs," "safe refusal," XML-only output

✅ **Long-context strategy**: Summary cap, repetition of key rules at end

✅ **Self-containment**: Evaluation criteria embedded (≤120 words, ≤2 tool calls, XML-safe)

## Gemini-Specific Best Practices Added

✅ **Few-shot examples**: Three consistent examples demonstrating expected analysis pattern [G1]

✅ **Clear, specific instructions**: Explicit task boundaries and output constraints [G1]

✅ **Response formatting**: Structured prefixes and XML template with clear completion pattern [G1]

✅ **Safety settings**: Granular harm category configuration with medium+ thresholds [G2]

✅ **Function calling**: AUTO mode with parallel calling capability and OpenAPI schema subset [G1]

✅ **Parameter optimization**: Temperature, top_p, top_k specified for consistent outputs [G1]