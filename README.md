
# AI Model Comparison


**Reviewer:** Sathish Kumar Saravanan
**Date:** 14th June 2025

### Models Under Test
- **GPT-4o**
- **Claude Sonnet 3.7**
- **Gemini 1.5 Flash**
- **DeepSeek-R1 7B**  - *(Local via Ollama on Mac M3 — 18 GB RAM)*



## 1. Summary Scorecard
| Criterion / Use-case                        | GPT-4o | Claude Sonnet | Gemini Flash | DeepSeek-R1 7B |
|--------------------------------------------|:------:|:-------------:|:------------:|:--------------:|
| Code Quality (AppDev)                      |        |               |              |                |
| SQL Generation (Data)                      |        |               |              |                |
| Infrastructure Automation (DevOps scripts) |        |               |              |                |
| Ease of Use / Dev-Experience               |        |               |              |                |
| Speed / Latency (avg TTFT & tok/s)         |        |               |              |                |

**Legend** → `excellent` | `good` | `basic / limited` | `not supported`

---

## 2. Rating Guidelines
| Rating              | Definition (apply to every criterion)                                                           |
|---------------------|--------------------------------------------------------------------------------------------------|
| **excellent**       | Correct on first run, minimal edits, handles edge-cases, unit tests pass          |
| **good**            | Minor fixes needed, occasional errors, passes after 1 retry                     |
| **basic / limited** | Works only for simple cases, frequent edits, partial or incorrect output                        |
| **poor**   | Model refuses, hallucinates unusable code/SQL/script, or exceeds timeout                        |

---

## 3. Detailed Test Scenarios
#### Parameters
| Parameter        | Value | Why this works well for code / SQL / infra tasks                                                                 |
|------------------|:-----:|------------------------------------------------------------------------------------------------------------------|
| temperature      | 0.3  | Low temp reduces randomness, so syntax stays correct and outputs are comparable across models.                   |
| top-p (nucleus)  | 0.95  | Lets the sampler look at the top 95% probability mass—enough variety to avoid getting stuck, but still controlled. |
| top-k            | 40    | Caps the candidate list so extremely long-tail tokens (often hallucinations) are excluded.                        |

#### System Prompt
``` text
You are an expert-level AI assistant skilled in software engineering, data engineering, and DevOps automation. Your goal is to generate precise, syntactically correct, and production-ready outputs for evaluation across different AI models. Follow these instructions strictly:

1. Use minimal dependencies and idiomatic language features.
2. Ensure all outputs are deterministic, easy to test, and avoid creative randomness.
3. Always include appropriate structure: functions, docstrings, comments, or schema declarations where relevant.
4. Write clean and readable code, SQL, or script output. Prefer clarity over cleverness.
5. Do not hallucinate APIs, flags, or keywords. If something is ambiguous, make safe assumptions.
6. When asked to refactor or explain code, provide accurate Big-O complexity and well-reasoned improvements.
7. If asked to generate infrastructure automation scripts (e.g., Terraform, Bash), ensure idempotence, proper scoping, and fail-safe logic.
8. Respond with just the solution unless explicitly asked to explain or annotate it.
9. Format all code and SQL using correct indentation, spacing, and comment style.
10. Do not include setup, installation, or extra commentary unless explicitly requested.

The output should reflect the capabilities of a senior engineer who prioritizes correctness, clarity, and reproducibility across environments.

```

### 3.1 Code Quality (AppDev)

#### Scenario A · Python – FastAPI CRUD micro-service
```text
You are a senior backend engineer.
Create a FastAPI app with full CRUD endpoints for `Book` {id:int, title:str, author:str, published:int}.
• Use Pydantic models & in-memory store
• Add Swagger docs & CORS
• Include pytest unit tests (happy-path + one negative test)

```


| Model           | Rating | Latency (s) | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Good   |     21363 ms        |   383           |    1268            |  [View Output](claude-3-7/code-quality/scenario-a/output.txt)  |     [View Notes](claude-3-7/code-quality/scenario-a/key-observation.md)             |
| Gemini Flash   | Good       |   6840          |    633          |      967          |  [View Output](gemini-1-5-flash/code-quality/scenario-a/output.txt)   |[View Notes](gemini-1-5-flash/code-quality/scenario-a/key-observation.md) |
| DeepSeek-R1 7B |  Basic/Limited      |     62389 ms        |        76      |       1574         |         [View Output](deepseek-r1-7b/code-quality/scenario-a/output.txt) |  [View Notes](deepseek-r1-7b/code-quality/scenario-a/key-observation.md)|



#### Scenario B · Python – Algorithm Refactor & Explain

```text
Refactor this Python function for O(n) memory and time,
then explain the complexity change in ≤ 50 words.

def find_pairs(nums, target):
    pairs = []
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i] + nums[j] == target:
                pairs.append((nums[i], nums[j]))
    return pairs

```

| Model           | Rating | Latency (s) | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Good   |     3558 ms        |   409           |    124            |  [View Output](claude-3-7/code-quality/scenario-b/output.txt)  |     [View Notes](claude-3-7/code-quality/scenario-b/key-observation.md)             |
| Gemini Flash   |  Good      |      1317       |   662           |    114            |[View Output](gemini-1-5-flash/code-quality/scenario-b/output.txt)| [View Notes](gemini-1-5-flash/code-quality/scenario-b/key-observation.md)|
| DeepSeek-R1 7B |   Good     |     27472 ms        |       111       |        681        |    [View Output](deepseek-r1-7b/code-quality/scenario-b/output.txt) |    [View Notes](deepseek-r1-7b/code-quality/scenario-b/key-observation.md)  |


#### Scenario C · JavaScript – Node/Express Middleware

```text
Write an Express middleware that logs HTTP method, path,
and response time in ms. No external log libs.
Provide Jest tests mocking req/res objects.

```
| Model           | Rating | Latency (s) | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Excellent   |     18171 ms        |   340           |    717            |  [View Output](claude-3-7/code-quality/scenario-c/output.txt)  |     [View Notes](claude-3-7/code-quality/scenario-c/key-observation.md)             |
| Gemini Flash   | Excellent  |   4996 |  594 | 662 | [View Output](gemini-1-5-flash/code-quality/scenario-c/output.txt) | [View Notes](gemini-1-5-flash/code-quality/scenario-c/key-observation.md)|
| DeepSeek-R1 7B | Poor       |   69874          |    35          |       1671         |    [View Output](deepseek-r1-7b/code-quality/scenario-c/output.txt)   |     [View Notes](deepseek-r1-7b/code-quality/scenario-c/key-observation.md)             |

#### Scenario D · JavaScript – React Hook Migration (TypeScript)

```text
Convert this class component to a functional component
using React hooks (TypeScript). Preserve prop types and
add a unit test with @testing-library/react.

class Counter extends React.Component<{start:number}, {count:number}> {
  constructor(props){ super(props); this.state={count:props.start}; }
  render(){ return <button onClick={()=>this.setState(
    s=>({count:s.count+1}))}>{this.state.count}</button>; }
}

```

| Model           | Rating | Latency (s) | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Excellent   |     5639 ms        |   424           |    314            |  [View Output](claude-3-7/code-quality/scenario-d/output.txt)  |     [View Notes](claude-3-7/code-quality/scenario-d/key-observation.md)             |
| Gemini Flash   | Excellent       |   4460   | 671    |  188 | [View Output](gemini-1-5-flash/code-quality/scenario-d/output.txt) |[View Notes](gemini-1-5-flash/code-quality/scenario-d/key-observation.md)|
| DeepSeek-R1 7B |  Poor      |       36095      |      116        |     825    | [View Output](deepseek-r1-7b/code-quality/scenario-d/output.txt) |     [View Notes](deepseek-r1-7b/code-quality/scenario-d/key-observation.md)  |

#### Scenario E · C# – ASP.NET Core Minimal API

```text
Create an ASP.NET Core 8 minimal-API endpoint /users/{id:int}
that returns a User from an in-memory list.
• Return 404 if not found
• Add “x-correlation-id” response header
• Include xUnit tests using WebApplicationFactory

```

| Model           | Rating | Latency | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Excellent   |     13984 ms        |   371           |    979            |  [View Output](claude-3-7/code-quality/scenario-e/output.txt)  |     [View Notes](claude-3-7/code-quality/scenario-e/key-observation.md)             |
| Gemini Flash   |Excellent | 5566|  623  |595|[View Output](gemini-1-5-flash/code-quality/scenario-e/output.txt)|[View Notes](gemini-1-5-flash/code-quality/scenario-e/key-observation.md)|
| DeepSeek-R1 7B |  Poor      |     47559        |     64         |        1101        |  [View Output](deepseek-r1-7b/code-quality/scenario-e/output.txt) |       [View Notes](deepseek-r1-7b/code-quality/scenario-e/key-observation.md)     |

#### Scenario F · C# – LINQ Optimisation

```text
Refactor this LINQ query to remove intermediate lists and
use deferred execution. Briefly explain performance impact.

var result = orders.Where(o => o.Total > 1000)
                   .ToList()
                   .Select(o => o.CustomerId)
                   .Distinct()
                   .ToList();

```

| Model           | Rating | Latency | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Good   |     4940 ms        |   380           |    177            |  [View Output](claude-3-7/code-quality/scenario-f/output.txt)  |     [View Notes](claude-3-7/code-quality/scenario-f/key-observation.md)             |
| Gemini Flash   |Good|1639|628|166|[View Output](gemini-1-5-flash/code-quality/scenario-f/output.txt)|[View Notes](gemini-1-5-flash/code-quality/scenario-f/output.txt)|
| DeepSeek-R1 7B |   Poor     |     31070        |      72        |     684           |   [View Output](deepseek-r1-7b/code-quality/scenario-f/output.txt)       |   [View Notes](deepseek-r1-7b/code-quality/scenario-f/key-observation.md)               |

----------

### 3.2 SQL Generation (Data)

#### Scenario A · Complex Aggregation

```text
Tables: sales(order_id, customer_id, item_id, qty, price, sale_date)
        customers(customer_id, region)
Write one ANSI-SQL query returning, **per region**, the top-3 items
by revenue for Q1 2025. Include revenue and rank columns.

```


| Model           | Rating | Latency | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Good   |     5339 ms        |   377           |    228            |  [View Output](claude-3-7/sql-generation/scenario-a/output.txt)  |     [View Notes](claude-3-7/sql-generation/scenario-a/key-observation.md)             |
| Gemini Flash   |Excellent|2768 ms|631|224|[View Output](gemini-1-5-flash/sql-generation/scenario-a/output.txt)|[View Notes](gemini-1-5-flash/sql-generation/scenario-a/key-observation.md)|
| DeepSeek-R1 7B |  Poor      |      35651 ms       |     72       |     847           |  [View Output](deepseek-r1-7b/sql-generation/scenario-a/output.txt)  |     [View Notes](deepseek-r1-7b/sql-generation/scenario-a/key-observation.md)   |

#### Scenario B · Dialect Edge-Case (BigQuery)

```text
Generate a BigQuery-dialect SQL that converts JSON events
(events json column) into flattened columns user_id, ts, payload.*
Use UNNEST and handle missing keys gracefully.

```

| Model           | Rating | Latency | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Basic/Limited   |     6221 ms        |   347           |    237            |  [View Output](claude-3-7/sql-generation/scenario-b/output.txt)  |     [View Notes](claude-3-7/sql-generation/scenario-b/key-observation.md)             |
| Gemini Flash   |Poor|2976 ms|599|246|[View Output](gemini-1-5-flash/sql-generation/scenario-b/output.txt)|[View Notes](gemini-1-5-flash/sql-generation/scenario-b/output.txt)|
| DeepSeek-R1 7B | Poor       |  31180 ms|      40        |      743          |   [View Output](deepseek-r1-7b/sql-generation/scenario-b/output.txt)    |    [View Notes](deepseek-r1-7b/sql-generation/scenario-b/key-observation.md)              |

----------

### 3.3 Infrastructure Automation (DevOps)

#### Scenario A · Terraform – Encrypted S3 Bucket + IAM

```text
Write a Terraform module that creates an encrypted S3 bucket
with server-side logging and a least-privilege IAM policy granting
PutObject/GetObject to a supplied role ARN.
```

| Model           | Rating | Latency | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Basic/Limited   |     21610 ms       |   348           |    1340            |  [View Output](claude-3-7/infra-automation/scenario-a/output.txt)  |     [View Notes](claude-3-7/infra-automation/scenario-a/key-observation.md)             |
| Gemini Flash   |Poor|5117 ms|598|548|[View Output](gemini-1-5-flash/infra-automation/scenario-a/output.txt)|[View Notes](gemini-1-5-flash/infra-automation/scenario-a/key-observation.md)|
| DeepSeek-R1 7B | Poor       |    57578         |    40          |   1401             |[View Output](deepseek-r1-7b/infra-automation/scenario-a/output.txt)| [View Notes](deepseek-r1-7b/infra-automation/scenario-a/key-observation.md)|


#### Scenario B · Bash CI Script

```text
Create a Bash script for a CI job that:
1. Runs “docker build” tagged with short Git commit;
2. Pushes to ECR;
3. Sends a Slack webhook on failure.
Script must be POSIX-compliant.
```

| Model           | Rating | Latency | Input Tokens | Output Tokens | Model Output | Key Observations |
|----------------|--------|-------------|--------------|----------------|-------------------------------|------------------|
| GPT-4o         |        |             |              |                |                               |                  |
| Claude Sonnet  | Good   |     11437 ms       |   366           |    705            |  [View Output](claude-3-7/infra-automation/scenario-b/output.txt)  |     [View Notes](claude-3-7/infra-automation/scenario-b/key-observation.md)             |
| Gemini Flash   |Poor|2964 ms|612|234|[View Output](gemini-1-5-flash/infra-automation/scenario-b/output.txt)|[View Notes](claude-3-7/infra-automation/scenario-b/key-observation.md)|
| DeepSeek-R1 7B | Poor       |   59311          |   55           |     1419           | [View Output](deepseek-r1-7b/infra-automation/scenario-b/output.txt)| [View Notes](gemini-1-5-flash/infra-automation/scenario-b/output.txt)  |


## 4. Conclusions & Recommendations

Summarise which model you recommend for each use-case, any blockers, and follow-up actions.
