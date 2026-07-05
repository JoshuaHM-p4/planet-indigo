# Planet Indigo: The Sentinel Sphere (Stateful Edition)

## Pre-Workshop Analysis Report

This report outlines the cost, engineering difficulty, process, and architectural shifts required to transition the AWS Intergalactic AI Shark Tank workshop from a stateless JSON endpoint to a stateful, multi-turn AI Agent experience.

---

### 1. Architectural Shift (Old vs. New)

**Old Architecture (Stateless)**

* **Workflow:** User submits form -> S3 -> API Gateway -> Lambda -> Bedrock `InvokeModel` -> Returns 3 static Shark quotes -> User reads and session ends.
* **State:** None. Lambda forgets the pitch immediately after responding.
* **AI:** Basic multi-persona prompt using `boto3`.

**New Architecture (Stateful & Agentic)**

* **Workflow:** User sends chat message -> S3 Chat UI -> API Gateway -> Lambda (Session Handler) -> **Amazon Bedrock Agent** -> Returns Shark responses + Wait for user reply.
* **State:** Managed by Amazon Bedrock Agents (or via DynamoDB if using Strands SDK). A `session_id` tracks the ongoing negotiation.
* **AI:** Stateful ReAct loop. AI remembers previous turns and adjusts its negotiation tactics.

---

### 2. Cost Analysis

Deploying multi-turn conversational agents increases costs due to repeated context window processing (the LLM has to re-read the chat history on every turn).

* **AWS Workshop Studio Base:** $0 (Covered by AWS if sponsored/approved).
* **Amazon Bedrock (Nova / Claude Haiku):**
  * *Stateless Cost:* ~1000 input tokens, ~500 output tokens per pitch. Tiny fractions of a cent per group.
  * *Stateful Cost:* Tokens grow exponentially as the chat gets longer. E.g., Turn 1: 1500 tokens. Turn 2: 2500 tokens. Turn 3: 3500 tokens.
  * *Estimate:* Even with 20 groups chatting for 2 hours, Bedrock costs should remain well under **$15-$30 total** for the entire cohort if using efficient models like Nova Micro or Claude 3 Haiku.
* **Amazon API Gateway & Lambda:** Free tier easily covers 30-50 users sending hundreds of messages.
* **Bedrock Agent Pricing:** Managed agents may incur slight orchestration premiums, but it is highly negligible for a 3-hour student workshop.

**Cost Conclusion:** Negligible risk if restricted to efficient models. High risk if students accidentally loop Claude 3.5 Sonnet infinitely. Strict IAM limits are required.

---

### 3. Engineering Difficulty (For Organizers)

**Rating: HIGH**

Moving to a stateful architecture shifts the heavy lifting from the students to the organizers. To ensure the 2-3 hour workshop is "fun" and frictionless, the organizer engineering team must prepare the following:

1. **Chat UI Template (Medium Effort):** Building a responsive React/Vanilla JS chat interface that manages `session_id` and arrays of messages.
2. **Stateful Backend Boilerplate (High Effort):** Writing a rock-solid AWS Lambda function that safely unpacks the API Gateway proxy event, injects the `session_id` into the `InvokeAgent` API, and formats the output into the strict multi-shark JSON contract.
3. **KiroCLI/Agent Templates (High Effort):** Writing the declarative YAML/JSON files required for KiroCLI or Harness so students can deploy their Agent infrastructure with a single terminal command.
4. **AWS Account Provisioning (High Effort):** Configuring IAM roles in 20 Workshop Studio accounts that allow Bedrock Agent creation (which requires complex trust policies).

---

### 4. Process (Workshop Flow)

With the gamified "Sharks vs. Entrepreneurs" mechanic, the workshop flows as follows:

* **Phase 1: The Build (90 minutes)**
  * *DAI / Shark Teams:* Focus entirely on AI. Use KiroCLI to deploy the Bedrock Agent. They spend the time aggressively tuning the multi-persona prompts (e.g., Dogecoin Dan, Brutal VC) to be tough negotiators.
  * *DCCI / Entrepreneur Teams:* Focus entirely on Cloud Infra. Deploy the S3 Frontend, build the API Gateway, and write the AWS Lambda function that will eventually call the Bedrock Agent.
* **Phase 2: The Integration (30 minutes)**
  * The Cross-Department Handshake: DAI teams hand over their `Agent ID` and `Alias ID`. DCCI teams inject this into their Lambda function, successfully bridging the DCCI frontend/network with the DAI AI backend.
* **Phase 3: The Shark Tank (60 minutes)**
  * **The Dynamic:** DCCI Offense vs. DAI AI Defense.
  * DCCI Entrepreneurs pitch their ideas using the Chat UI they hosted, which connects to the DAI group's Shark AI.
  * The AI Sharks respond, asking tough follow-up questions.
  * Entrepreneurs scramble to defend their startups in the chat in a timed 5-minute round.
  * **Scoring/Win Conditions:**
    * *Entrepreneurs* win by successfully negotiating a `"status": "FUNDED"` from the AI. They are tracked on a central leaderboard by Total Capital Raised.
    * *Sharks* win by having their AI successfully grill the entrepreneurs and defend their capital without breaking character or formatting.
  * *(Bonus Idea: AI vs AI auto-battler where Entrepreneur teams also build a "Salesperson Agent" and both AIs debate autonomously).*

---

### 5. Organizer Tutorial: How to Prepare

Follow these steps to prepare the technical assets 2 weeks before the event:

**Step 1: Build the 'Sentinel-Sphere-Starter' GitHub Repo**

* Create `/frontend` containing `index.html`, `style.css`, and a stateful `app.js` chat script.
* Create `/backend` containing the `lambda_function.py` boilerplate. Use the `boto3` Bedrock Agent Runtime client.

**Step 2: Author the KiroCLI Agent Template**

* Create a `shark_agent.yaml` file that defines the Bedrock Agent.
* Define the Agent Resource Role, Foundation Model (Nova), and the base System Instruction.
* Leave `TODO: Insert your brutal Shark Persona prompts here` inside the YAML for the students to edit.

**Step 3: Secure the AWS Environment**

* If using AWS Workshop Studio, request 20-25 accounts.
* Create an IAM policy template that Grants:
  * `bedrock:CreateAgent`
  * `bedrock:InvokeAgent`
  * `lambda:CreateFunction`
  * `apigateway:*`
  * `s3:*`
* Block access to expensive EC2 instances or unapproved Bedrock models (like Claude 3 Opus).

**Step 4: Dry Run**

* The organizer team must do a full 1-hour dry run from an empty AWS account. If it takes the organizers more than 30 minutes to deploy the templates, it will take the students 3 hours. Simplify until the deployment takes 5 minutes.
