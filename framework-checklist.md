# LLM Framework Evaluation Checklist

Quick reference for evaluating LLM frameworks against AICE standards.

## Must Haves
- [ ] Supports Amazon Bedrock (Inference Profiles, Guardrails)
- [ ] Supports Azure AI Foundry (Managed Identity authentication)
- [ ] Open source with active maintenance and support
- [ ] Emits OpenTelemetry (aligns with GenAI semantic conventions)
- [ ] Transparent abstractions with direct model access when needed
- [ ] No enforced architectural patterns (supports simple to agentic use cases)
- [ ] Lightweight dependencies (no bloat for vector DBs, orchestration, etc.)
- [ ] Tool calling via internal functions AND Model Context Protocol (MCP)

## Should Haves
- [ ] Extensible with custom model providers
- [ ] Supports invoking embedding models

## Nice to Haves
- [ ] Supports both Python and Node.js
- [ ] Built-in LLM usage limits (tokens, tool calls, request rate)
- [ ] Structured output parsing by object schema

---

**For detailed evaluation criteria and philosophy, see [README.md](README.md#our-checklist-for-evaluating-ai-frameworks).**
