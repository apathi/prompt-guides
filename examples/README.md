# Examples

This directory contains practical implementations of the prompt engineering best practices covered in this guide.

## Available Examples

### 🏆 [CAPSTONE: Compliance Analyzer](./capstone_compliance_analyzer.md)

A comprehensive example that implements **all best practices** from the guide in a real-world compliance analysis scenario:

- System prompts with persistent roles
- Agentic tool preambles (OpenAI-style)
- XML-structured prompts (Anthropic-style)
- Safety budgets and guardrails
- Structured output formatting
- Long-context strategies
- Self-contained evaluation criteria

This example shows how to:
- Extract KPIs from policy documents
- Flag compliance anomalies with severity levels
- Use tools safely with explicit budgets
- Generate structured XML output
- Handle uncertainty and missing data

## Provider-Specific Approaches

Each provider has distinct characteristics that influence how prompts should be structured and optimized:

### Quick Comparison

| Aspect | OpenAI GPT-5 | Anthropic Claude | Google Gemini |
|--------|--------------|------------------|---------------|
| **Structure** | Tool preambles + delimiters | XML segmentation | Few-shot examples + prefixes |
| **Planning** | `reasoning_effort` levels | Chain of thought + multishot | Response patterns + validation |
| **Safety** | Parameter constraints | Guardrail patterns + JSON mode | Category thresholds + filters |
| **Tools** | Plan → act → narrate workflow | `tool_use` ↔ `tool_result` loops | AUTO/ANY/NONE function modes |
| **Output** | System prompts + structured | Bookend pattern + consistency | Clear instructions + formatting |

### 🤖 OpenAI GPT-5 Characteristics

- **🎯 Tool Preambles**: Plan → act → narrate → stop workflow for transparent agent behavior
- **🧠 reasoning_effort**: Adjustable depth (minimal/low/medium/high) to balance latency vs thoroughness
- **👤 System Prompts**: Clear separation between system role and user instructions
- **📐 Delimiters**: Uses `"""`, `###` for clean section separation
- **🔄 Agentic Flows**: Multi-turn planning, execution, and verification patterns
- **⚙️ Parameter Control**: Fine-tuned temperature, max_tokens, and reasoning depth

**Best For**: Complex multi-step tasks requiring planning, reasoning depth control, and transparent agent workflows.

### 🎭 Anthropic Claude Characteristics

- **📋 XML Segmentation**: `<instructions>`, `<context>`, `<format>` for clear structure
- **🔧 Tool Interaction**: Explicit `tool_use` ↔ `tool_result` conversation loops
- **🎯 Multishot Prompting**: Few-shot examples combined with chain of thought reasoning
- **🔒 Bookend Pattern**: Instructions first, critical reminders last for long-context stability
- **📊 JSON Mode**: Structured output consistency with schema enforcement
- **🛡️ Safety Guardrails**: Reduce hallucinations, prevent prompt leaks, increase consistency

**Best For**: Structured analysis tasks, long-context processing, safety-critical applications requiring explicit guardrails.

### 🔍 Google Gemini Characteristics

- **📝 Few-Shot Emphasis**: 2-3 consistent examples preferred over zero-shot approaches
- **📏 Clear Instructions**: Explicit task boundaries, constraints, and output specifications
- **🎨 Response Formatting**: Structured prefixes (input/output/example) and completion patterns
- **🛡️ Safety-First Design**: Granular settings across 5 harm categories with configurable thresholds
- **⚡ Function Calling**: AUTO/ANY/NONE modes supporting parallel and compositional calling
- **🎛️ Parameter Optimization**: Temperature, top_p, top_k tuning for consistent, focused outputs

**Best For**: Tasks requiring consistent formatting, safety-sensitive applications, and scenarios where few-shot learning improves performance.

## Usage Notes

Each example includes:
- **Provider-specific implementations** (OpenAI GPT-5, Anthropic Claude, Google Gemini)
- **Complete prompt code** with parameters and API configurations
- **Expected output** samples
- **Best practices checklist** showing which techniques are demonstrated
- **Provider-specific best practices** highlighting unique approaches

These examples are designed to be copy-paste ready for your own projects while illustrating the principles covered in the main guide.