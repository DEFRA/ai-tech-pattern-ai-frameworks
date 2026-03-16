# LLM Framework Technical Pattern
This repository contains guidelines, best practices, and recommendations for using LLM library and/or frameworks for invoking LLMs in production applications.

## Who this pattern is for
- Developers building services that use LLMs as a core component.

## The case for using LLM libraries / frameworks
It's important to remember that LLMs are at their core, are just powerful predictive models; predicting the next token in the sequence. Owing to this simplicity, a lot of developers opt to simply call the model's API directly, without using a library or framework.

This is a perfectly valid approach for simple use cases and also gives the developer maximum control over the request. However, this approach will result in developers trying to solve a lot of the same problems over and over again, such as:
* Message formatting (e.g. system, user, assistant roles)
* Model specific response/request standards (e.g. OpenAI API vs Anthropic Messages API)
* Different API endpoints for different model types (e.g. chat vs completion)
* Vendor specific parameters (e.g. temperature, top_p, etc.)
* Specific patterns for invoking tools (e.g. function calling in OpenAI, tool calls in Azure AI Foundry, etc.)

Owing to the model specific nuances, using model APIs / SDKs directly can also cause friction if the team ever needs to switch the model provider, or even just the model itself (e.g. switching from a completion model to a chat model). This friction is further amplified in multi-agent workflows, or where end-users are allowed to select from a variety of models, as the code to support this can quickly become complex and difficult to maintain.

Our approach is to favour lightweight, library-style packages that provide focused abstractions for the problems above, without pulling in unrelated capabilities. Heavier frameworks such as LangChain bundle additional features — agentic orchestration, vector database integrations, and more. These can be valuable in the right context (such as prototyping) but they tend to make assumptions about your:
- Architectural patterns
- Underlying infrastructure (IAM permissions, single-service db ownership, etc.)

In our experience, when using heavier frameworks, you end up fighting the framework (and its abstractions) more often than the framework helps you. Frameworks should make your life easier, not harder.

## When to use an LLM framework
We recommend that most teams should use one of our [recommended frameworks](#our-recommendations) when building production applications that use LLMs. Our recommendations should be suitable for most use cases - ranging from simple LLM calls to complex agentic applications.

The only time you should consider not using one of our recommended frameworks is if at least one of the following applies:
- You need to use a specific feature of a model provider that is not yet supported by any framework (e.g. a new feature in Amazon Bedrock that is not yet supported by Pydantic AI)
- You are using a model provider that is not yet supported by any framework - **Important**: if this is the case, we encourage you to explore extending one of the existing frameworks to support the provider
- Your language of choice is not covered in our recommendations

> [!WARNING]
> If none of our recommended frameworks meet your needs, you **MUST NOT** use an unapproved framework without consulting the AI Capabilities and Enablement (AICE) team. In these cases, we will likely recommend that you use vendor APIs / SDKs directly, but we want to ensure that you have explored all options before going down this path.

## Our checklist for evaluating AI frameworks
The AICE team has developed a checklist to evaluate LLM frameworks based on the following criteria:

### Foundation & Design
- **MUST** be open source and have active maintenance and support
- **DESIRABLE** the framework supports both Python and Node.js
- **MUST** Any abstractions provided by the framework must be transparent and allow for direct model access when needed, without forcing the use of other framework features.
- **MUST** Not enforce a specific architectural pattern (e.g. agentic) and should be flexible enough to support a variety of use cases, from simple LLM calls to complex agentic applications.
- **MUST** Not have a heavy dependency footprint, and should not pull in unrelated capabilities that are not directly related to invoking LLMs and handling their responses (e.g. vector database integrations, agent orchestration, etc.)
- **SHOULD** support extending the framework with custom model providers

### Provider Support & Observability
- **MUST** support at least the following LLM hosting providers:
    - Amazon Bedrock
        - Inference Profiles
        - Guardrails
    - Azure AI Foundry
      - Managed Identity authentication
- **MUST** support emitting OpenTelemetry in-line with [Semantic conventions for generative AI systems](https://opentelemetry.io/docs/specs/semconv/gen-ai)
- **DESIRABLE** it has built-in LLM usage limits, including but not limited to:
    - Token usage limits
    - Tool call limits
    - LLM request rate limits

### Tool Calling & Integration
- **MUST** support LLM tool calling via both:
    - Internal application functions
    - Model Context Protocol (MCP) servers

### Model Capabilities
- **SHOULD** support invoking embedding models
- **DESIRABLE** it supports structured output parsing according to object schemas

## Our recommendations

### Python
- ⭐ [Pydantic AI](https://ai.pydantic.dev)

### Node.js

> [!NOTE]
> The AICE team does not currently have a recommendation for Node.js. If you are building a Node.js service, please consult the AICE team for guidance.
