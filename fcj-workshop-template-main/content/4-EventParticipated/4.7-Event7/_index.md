---
title: "Event 7"
date: "`r Sys.Date()`"
weight: 407
chapter: false
pre: " <b> 4.7. </b> "
---

# AWS Bedrock Agentic AI Workshop Event Report

---

## Executive Summary

I attended a comprehensive half-day workshop on Agentic AI using Amazon Bedrock at the AWS Vietnam Office. The session covered the architecture and implementation of AI agents from foundational concepts through hands-on development. The workshop was particularly valuable for understanding how to build production-grade agentic systems that can autonomously orchestrate complex workflows, make decisions, and integrate with external tools. While my Cloud Health Dashboard currently uses traditional monitoring approaches, the concepts presented here open possibilities for AI-powered security analysis, automated remediation recommendations, and intelligent anomaly detection.

---

## Opening Session: Agentic AI Landscape (9:00 - 9:10 AM)

Nguyen Gia Hung, Head of Solutions Architect, framed the workshop by distinguishing between traditional AI applications and agentic systems. The key difference: **agency** - the ability of AI systems to perceive environments, make decisions, take actions, and learn from outcomes without constant human intervention.

---

## AWS Bedrock Agent Core Architecture (9:10 - 9:40 AM)

Kien Nguyen's technical deep dive into Bedrock Agents revealed the underlying architecture that makes autonomous AI systems possible.

**What is Amazon Bedrock?**
Bedrock is AWS's fully managed service for foundation models (FMs) from providers like Anthropic (Claude), Meta (Llama), Cohere, AI21, and Amazon's own Titan models. Key differentiators:
- **No infrastructure management:** No EC2 instances, no model hosting, no scaling concerns
- **Model choice flexibility:** Switch between models via API without code changes
- **Security & compliance:** Models run within AWS infrastructure, data doesn't leave your VPC
- **Fine-tuning & customization:** Train models on proprietary data while maintaining data privacy

**Bedrock Agents - The Core Concept:**

Agents are autonomous systems that can:
1. **Understand user intent** through natural language
2. **Break down complex requests** into executable steps (reasoning)
3. **Select and invoke appropriate tools** (APIs, Lambda functions, knowledge bases)
4. **Orchestrate multi-step workflows** dynamically
5. **Return structured responses** with citations and reasoning traces

The architecture diagram showed:

```
User Request
    ↓
Bedrock Agent (Orchestrator)
    ↓
┌─────────────┬──────────────┬─────────────┐
│ Foundation  │  Action      │  Knowledge  │
│ Model       │  Groups      │  Bases      │
│ (Claude)    │  (Tools)     │  (RAG)      │
└─────────────┴──────────────┴─────────────┘
    ↓
Response with reasoning trace
```

**Action Groups - Extending Agent Capabilities:**

Action groups define what an agent can *do*. Each action group contains:
- **OpenAPI schema:** Defines available functions, parameters, return types
- **Lambda function:** Backend logic that executes the action
- **Description:** Helps the model understand when to use this action


When a user asks "What should I do about this high-severity GuardDuty finding?", the agent:
1. Recognizes it needs security analysis capability
2. Selects `analyzeGuardDutyFinding` function
3. Invokes Lambda with finding details
4. Receives structured response
5. Formulates human-readable recommendation

**Knowledge Bases - RAG Integration:**

Knowledge bases allow agents to query proprietary documents using semantic search. The workflow:

1. **Ingestion:** Documents (PDFs, web pages, text files) stored in S3
2. **Chunking:** Documents split into smaller segments (1000 tokens default)
3. **Embedding:** Each chunk converted to vector embeddings using Titan Embeddings
4. **Storage:** Vectors stored in OpenSearch Serverless (or other vector DBs)
5. **Retrieval:** Agent queries knowledge base, gets most relevant chunks
6. **Generation:** Model generates response using retrieved context

For my dashboard, I could create knowledge bases containing:
- AWS security best practices documentation
- CIS Benchmark requirements
- Internal security runbooks
- Past incident reports

When users ask "How should I configure VPC Flow Logs?", the agent would retrieve relevant documentation chunks and provide contextualized answers specific to their environment.

**Guardrails - Responsible AI:**

Bedrock Guardrails ensure agents behave safely:
- **Content filters:** Block harmful, biased, or inappropriate outputs
- **Denied topics:** Prevent discussion of specified subjects
- **Word filters:** Block profanity or sensitive terms
- **PII redaction:** Automatically mask sensitive information
- **Hallucination detection:** Reduce factually incorrect responses

For production deployment, guardrails are essential to prevent agents from suggesting dangerous remediation actions (like deleting production databases).

**Prompt Engineering for Agents:**

The session revealed that agent prompts differ from standard LLM prompts. Agent prompts must:
- Define agent role and responsibilities clearly
- Specify how to use tools (when to call, what parameters to pass)
- Provide response formatting instructions
- Include examples of correct tool usage
- Set boundaries (what NOT to do)

Example agent instruction for security analysis:

```
You are a cloud security analyst specializing in AWS environments. Your role is to:
1. Analyze security findings from GuardDuty, Security Hub, and IAM Access Analyzer
2. Prioritize findings by severity and business impact
3. Recommend specific, actionable remediation steps
4. Explain the risk context in business terms

When analyzing findings:
- Always check the severity level first
- Use the analyzeGuardDutyFinding tool for detailed analysis
- Cross-reference with AWS security best practices knowledge base
- Provide both immediate containment steps and long-term preventive measures

Do NOT:
- Make changes to production resources without explicit user confirmation
- Suggest deleting resources without understanding their purpose
- Provide generic recommendations without environment-specific context
```

---

## Use Case: Building Agentic Workflow on AWS (9:40 - 10:00 AM)

Viet Pham, Founder & CEO of an AI startup, shared a real-world implementation of agentic workflows in production. The use case involved building a customer support automation system for an e-commerce platform.

**The Problem:**
Traditional chatbots follow decision trees - rigid, limited, and break when users ask unexpected questions. The company needed a system that could:
- Understand complex, multi-intent customer queries
- Access order databases, inventory systems, shipping APIs
- Make intelligent decisions (refund vs. exchange vs. escalate)
- Handle conversations across multiple turns
- Maintain context throughout the interaction

**The Solution - Agentic Workflow Architecture:**

```
Customer Query → Bedrock Agent → Orchestration Logic
                                        ↓
                 ┌──────────────────────┼──────────────────────┐
                 ↓                      ↓                       ↓
            Order Lookup          Inventory Check        Shipping API
            (Lambda + RDS)        (Lambda + DynamoDB)    (La   ↓mbda + 3rd-party)
                 ↓                      ↓                    
                 └──────────────────────┴───────────────────────┘
                                        ↓
                                 Agent Decision Making
                                        ↓
                              Response + Action Taken
```

**Agent Workflow Example:**

User: "I ordered a laptop 3 days ago but haven't received shipping confirmation. Can I change the shipping address?"

Agent reasoning trace (shown in the demo):
1. **Intent Recognition:** Order status query + shipping address modification
2. **Tool Selection:** Call `getOrderStatus` with order ID (extracted from customer session)
3. **Tool Execution:** Lambda queries RDS, returns order details
4. **Analysis:** Order in "processing" state, not yet shipped
5. **Decision:** Address modification is possible
6. **Tool Selection:** Call `updateShippingAddress`
7. **Confirmation:** Ask user for new address before executing
8. **Tool Execution:** Update address in database
9. **Response Generation:** Confirm change + provide estimated shipping date

The beauty of this approach: **no hardcoded decision trees**. The agent dynamically determines workflow based on real-time data and user intent.

**Multi-Step Reasoning Pattern:**

The demo showed how agents handle complex queries requiring multiple API calls:

User: "What's the cheapest laptop under $1000 that can be delivered by Friday?"

Agent workflow:
1. **Query 1:** `searchProducts(category="laptop", max_price=1000)` → Returns 15 products
2. **Query 2:** `checkInventory(productIds=[...])` → Returns in-stock items
3. **Query 3:** `getShippingEstimate(productIds=[...], targetDate="Friday")` → Returns delivery dates
4. **Synthesis:** Agent compares prices and delivery dates, ranks options
5. **Response:** "The HP Pavilion 15 at $899 can arrive Thursday. Would you like to proceed?"

This multi-step reasoning is what makes agents "intelligent" - they don't just execute predefined workflows, they construct workflows on the fly.

**Error Handling & Fallbacks:**

A critical part of the presentation covered what happens when tools fail:

- **API timeout:** Agent retries with exponential backoff, or suggests alternative action
- **Invalid parameters:** Agent reformulates request with corrected parameters
- **Ambiguous intent:** Agent asks clarifying questions rather than guessing
- **Out-of-scope request:** Agent escalates to human agent gracefully

The error handling wasn't hardcoded - the agent's foundation model reasoning enabled it to adapt to failures contextually.

**Production Metrics Shared:**
- **Query resolution rate:** 78% fully automated (vs. 35% with traditional chatbot)
- **Average response time:** 4.2 seconds for multi-step queries
- **Customer satisfaction:** 4.1/5 (vs. 3.2/5 for previous system)
- **Cost:** ~$0.03 per conversation (Bedrock API + Lambda execution)

**Key Takeaway for My Project:**
The multi-step reasoning pattern applies directly to security incident investigation. When a GuardDuty finding appears, an agent could: (1) Fetch finding details, (2) Query CloudTrail for related API calls, (3) Check IAM policies for affected resources, (4) Retrieve similar past incidents from knowledge base, (5) Generate contextual remediation plan. This transforms my dashboard from reactive (show finding) to proactive (investigate and recommend).

---

## CloudThinker Introduction (10:00 - 10:10 AM)

Thang Ton, Co-founder & COO of CloudThinker, introduced their platform - an agentic orchestration layer built on top of Amazon Bedrock. CloudThinker addresses pain points in building production agentic systems:

**Problems with Raw Bedrock Agents:**
1. **Limited observability:** Hard to debug why agent made specific decisions
2. **Context window management:** Agents lose context in long conversations
3. **Multi-agent coordination:** No native support for agent-to-agent communication
4. **Prompt versioning:** No built-in system for A/B testing prompts
5. **Cost optimization:** Difficult to minimize token usage without sacrificing quality

**CloudThinker's Value Proposition:**

**1. Agentic Orchestration Layer:**
- **Multi-agent workflows:** Coordinate specialist agents (data analyst agent + report writer agent)
- **Agent handoff:** Seamlessly transfer context between agents
- **Parallel execution:** Run multiple agents simultaneously, synthesize results
- **Conditional routing:** Route to appropriate agent based on query type

**2. Context Optimization:**
- **Intelligent caching:** Reuse embeddings for repeated queries
- **Dynamic context pruning:** Keep only relevant conversation history
- **Hierarchical memory:** Short-term (current session) + long-term (across sessions)
- **Semantic compression:** Summarize old context to save tokens

**3. Observability & Debugging:**
- **Agent reasoning traces:** See every decision, tool call, and model invocation
- **Cost tracking:** Per-conversation, per-agent, per-tool usage metrics
- **Latency breakdown:** Identify bottlenecks (embedding, retrieval, generation)
- **A/B testing framework:** Compare prompt variations systematically

The platform demo showed a dashboard (meta: a dashboard for building my dashboard's AI agent) with:
- Real-time agent execution visualization
- Token usage graphs
- Tool invocation frequency
- Success/failure rates per agent

**Pricing Model:**
CloudThinker charges based on orchestration complexity, not token usage:
- Free tier: 1,000 agent executions/month
- Pro: $99/month for 50,000 executions
- Enterprise: Custom pricing with dedicated support

Since Bedrock costs are separate, this could be cost-effective for high-volume agent workloads where orchestration optimization reduces overall Bedrock token usage.

**Key Takeaway for My Project:**
If I build an AI agent for my dashboard, CloudThinker could help with observability and multi-agent coordination (e.g., one agent for GuardDuty analysis, another for IAM policy review, coordinated by orchestrator). However, for a student project, starting with raw Bedrock Agents makes more sense to understand fundamentals before adding abstraction layers.

---

## CloudThinker Agentic Orchestration Deep Dive (10:10 - 10:40 AM)

Henry Bui, Head of Engineering, provided a technical deep dive into CloudThinker's orchestration engine. This session was highly advanced (L300 level) and revealed sophisticated patterns for production agentic systems.

**Context Window Management - The Core Challenge:**

Foundation models have finite context windows:
- Claude 3.5 Sonnet: 200K tokens (~150K words)
- GPT-4 Turbo: 128K tokens
- Llama 3.1: 128K tokens

In long conversations or complex workflows, context can exceed these limits. Traditional approaches:
- **Truncation:** Drop old messages (loses context)
- **Summarization:** Compress old context (loses details)
- **Sliding window:** Keep recent N messages (loses distant but relevant context)

**CloudThinker's Hierarchical Memory System:**

```
┌─────────────────────────────────────────┐
│   Working Memory (Current Session)      │
│   - Last 10 conversation turns          │
│   - Active tool results                 │
│   - Temporary variables                 │
│   Token Budget: 20K tokens              │
└─────────────────────────────────────────┘
              ↕ (relevance-based retrieval)
┌─────────────────────────────────────────┐
│   Episodic Memory (Past Sessions)       │
│   - Summarized conversation history     │
│   - Key decisions & outcomes            │
│   - User preferences learned            │
│   Stored in: DynamoDB + Vector DB       │
└─────────────────────────────────────────┘
              ↕ (semantic similarity search)
┌─────────────────────────────────────────┐
│   Semantic Memory (Long-term Knowledge) │
│   - Domain facts                        │
│   - Procedures & best practices         │
│   - Tool usage patterns                 │
│   Stored in: Knowledge Base (OpenSearch)│
└─────────────────────────────────────────┘
```

**How It Works:**

1. **New user query arrives** → Goes into Working Memory
2. **Relevance scoring:** Semantic search across Episodic Memory for related past context
3. **Dynamic loading:** Only relevant past context loaded into Working Memory
4. **Token budget enforcement:** If over limit, least relevant context pruned
5. **After response:** Conversation turn compressed and stored in Episodic Memory

Example for my security dashboard:

User today: "Show me recent GuardDuty findings"
→ Agent retrieves findings, stores in Working Memory

User later: "Are any of these related to the credential exposure incident from last month?"
→ Agent searches Episodic Memory for "credential exposure" context
→ Loads relevant findings from that incident
→ Compares with current findings
→ Responds with correlation analysis

This enables long-term memory without blowing up context windows.

**Multi-Agent Orchestration Patterns:**

CloudThinker supports three agent collaboration patterns:

**1. Sequential (Pipeline):**
```
User Query → Agent A (classifier) → Agent B (executor) → Agent C (formatter) → Response
```
Example: Query classification → Data retrieval → Report generation

**2. Parallel (Fan-out/Fan-in):**
```
                  ┌→ Agent A (GuardDuty analysis)
User Query → Router ┼→ Agent B (IAM review) ┼→ Aggregator → Response
                  └→ Agent C (VPC Flow analysis)
```
Example: Comprehensive security assessment across multiple services

**3. Conditional (Decision Tree):**
```
User Query → Router → IF security_finding:
                         Agent A (incident response)
                      ELIF billing_alert:
                         Agent B (cost optimization)
                      ELSE:
                         Agent C (general assistance)
```

The demo showed a **fraud detection use case** with parallel agents:

```
Suspicious Transaction Detected
         ↓
    Orchestrator
         ↓
   ┌─────┴──────┬────────┐
   ↓            ↓        ↓
Account      Behavior  External
History      Pattern   Threat
Agent        Agent     Intelligence
   ↓            ↓        ↓
 "Normal    "Anomalous  "IP from
  user      login       known
  pattern"  location"   fraud list"
   └─────┬──────┴────────┘
         ↓
    Aggregator Agent
    (Risk Scoring)
         ↓
    Block transaction + Alert security team
```

Each agent runs in parallel, results aggregated, decision made in ~3 seconds.

**For my Cloud Health Dashboard, this pattern enables:**

User: "Analyze the security posture of account X"
```
Orchestrator
    ↓
┌───┴───┬─────────┬────────┐
↓       ↓         ↓        ↓
GuardDuty IAM     VPC      Data
Analysis  Review  Config   Encryption
Agent     Agent   Agent    Agent
    └───┬───┴─────────┴────────┘
        ↓
   Report Generation Agent
        ↓
   Comprehensive Security Assessment
```

**Prompt Chaining & Self-Refinement:**

Advanced pattern where agent critiques and improves its own output:

```
Step 1: Generate initial response
Step 2: Critique response (separate agent or same agent with critic role)
Step 3: Refine based on critique
Step 4: Validate against criteria
Step 5: Return final response
```

Demo showed code generation example:
1. Agent generates Python function
2. Critic agent reviews for security issues, edge cases, efficiency
3. Original agent refines code based on feedback
4. Validator checks if code passes test cases
5. Iterations continue until validation passes (max 3 iterations)

This self-refinement dramatically improves output quality, especially for complex tasks like IAM policy generation or CloudFormation template creation.

**Cost Optimization Techniques:**

Henry shared production metrics showing CloudThinker's optimizations:

**Without optimization:**
- Average conversation: 45K tokens
- Cost per conversation: $0.15 (Claude Sonnet)
- 10,000 conversations/month: $1,500

**With CloudThinker optimization:**
- Average conversation: 18K tokens (60% reduction)
- Cost per conversation: $0.06
- 10,000 conversations/month: $600

**Optimization techniques used:**
1. **Semantic caching:** Cache embeddings for repeated queries (50% cache hit rate)
2. **Lazy loading:** Only load knowledge base chunks when needed
3. **Compression:** Summarize old context aggressively
4. **Model routing:** Use cheaper models (Haiku) for simple queries, Sonnet for complex
5. **Batch processing:** Group similar queries to reuse context

**Key Takeaway for My Project:**
The hierarchical memory system and multi-agent orchestration patterns are production-critical for any agentic system at scale. For my dashboard, starting with a single agent is appropriate, but architecting for future multi-agent expansion (separate agents for GuardDuty, IAM, Network, Data protection) ensures scalability. The cost optimization techniques are essential if I add AI features - even with AWS credits, efficient token usage matters.

---

## Hands-On Workshop: Building a Bedrock Agent (11:00 - 12:00 PM)

Kha Van led the practical workshop where we went to cloudthinker.io to optimaze cost, security and provide recommendations.

---

## Conclusion & Reflection

The AWS Bedrock Agentic AI workshop transformed my understanding of how AI can move from passive tools (chatbots) to active systems (agents). The five key layers of agentic systems—foundation models, tool integration, knowledge bases, orchestration, and guardrails—create capabilities that go far beyond traditional AI applications.

**Most Valuable Insights:**

1. **Agents enable new UX patterns:** Instead of users figuring out which dashboard section to click or which API to call, they ask natural language questions and the agent orchestrates the solution. This dramatically lowers the barrier to cloud security expertise.

2. **Tool calling is the breakthrough:** The ability for LLMs to reliably call external functions (APIs, databases, AWS services) with correct parameters turns language models from text generators into task executors.

3. **Knowledge bases solve the freshness problem:** Instead of retraining models or hardcoding rules, knowledge bases let agents access up-to-date information (AWS documentation, security best practices) through semantic search.

4. **Multi-agent orchestration enables specialization:** Rather than one monolithic agent trying to handle everything, specialized agents (GuardDuty expert, IAM expert, VPC expert) can collaborate, each leveraging focused knowledge.

5. **Production requires observability:** The reasoning trace feature is essential for debugging, compliance, and continuous improvement. Without visibility into agent decision-making, production deployment is reckless.

**Personal Learning Outcomes:**

1. **Technical Depth:** Gained hands-on experience with Bedrock Agents, action groups, knowledge bases, and OpenAPI schema design. The workshop code examples provide a clear template for my implementation.

2. **Architectural Thinking:** Understanding the tradeoffs between single-agent vs. multi-agent, synchronous vs. asynchronous, and real-time vs. batch processing will inform my dashboard design decisions.

3. **Production Mindset:** The emphasis on observability, cost optimization, and guardrails reinforced that production AI systems require infrastructure beyond the model itself—logging, monitoring, safety controls.

4. **AWS Service Integration:** Learned how Bedrock integrates with broader AWS ecosystem (Lambda, S3, OpenSearch, CloudWatch, IAM). This reinforces my understanding of AWS service orchestration.

---