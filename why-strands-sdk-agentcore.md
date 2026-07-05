### 1. Do we still need Bedrock Agentcore + Strands SDK?

  Yes, they go from being "overkill" to being the "perfect fit."

  When building a stateful AI application, the biggest challenge is Memory Management. AWS Lambda is stateless; it forgets everything the moment it
  finishes executing. If an Entrepreneur replies to an AI Shark's question, the Lambda function needs to remember the original pitch and what the
  Shark asked.

  • Why Strands SDK / Agentcore fits: These frameworks abstract away the heavy lifting of state management, conversation loops, and context window
  limits.
  • Alternative (The Hard Way): If you don't use these SDKs and just stick to basic  boto3 , you would have to manually provision an Amazon DynamoDB
  table to store the chat history, and write complex Lambda logic to fetch, append, and pass the entire conversation history back to Bedrock on
  every single API call.

  Recommendation: Lean heavily into Amazon Native Bedrock Agents. It manages the session state (memory) automatically just by passing a  SessionId .
  ──────

### 2. What do we need to do to support the Game Functions?

  To support the game functions (Sharks vs. Entrepreneurs, Group Accounts, Templates) with a stateful design, the workshop structure changes to
  this:

  • The Roles:
      • Shark Teams: Their task is to engineer the "meanest, most perceptive" AI Shark personas using the Agent templates. They deploy the Bedrock
      Agents and the Lambda/API Gateway backend.
      • Entrepreneur Teams: Their task is to come up with a pitch, connect to a Shark Team's API, and use the frontend UI to survive a multi-turn
      negotiation with the AI Sharks.
  • The Interaction: Instead of a single "Submit Pitch" button, the Frontend template must now be a Chat Interface. The Entrepreneur types their
  pitch, the AI Sharks reply with probing questions, the Entrepreneur defends their idea, and eventually, the AI Sharks render a "FUNDED" or
  "REJECTED" decision.
  ──────

### 3. How much engineering do we need to do to set it up?

  Moving to a stateful, multi-turn architecture significantly increases the engineering preparation required by our team. Here is what we must build
  before the workshop starts:

  A. The Frontend Chat Template (High Effort)
  We can no longer use a simple web form. We need to build a lightweight, polished web-based Chat UI (HTML/Vanilla JS/CSS) that can maintain a
  session and append messages to a chat window. We will provide this fully built; the students just host it on S3.

  B. The Backend & State Management Boilerplate (High Effort)
  We must choose our stateful path and build the starter code:

  • If using Native Bedrock Agents: We need to provide a Lambda template that simply takes the frontend input and a  SessionId , calls the
  InvokeAgent  API, and returns the response.
  • If using Strands SDK + DynamoDB: We need to provide the CloudFormation templates to spin up the DynamoDB tables in their AWS accounts, and the
  Lambda boilerplate that handles the Strands SDK database connection.

  C. CLI and Agent Templates (Medium Effort)
  Since the suggestions encourage using KiroCLI/Harness, we need to pre-author the Agent configuration files ( agent.yaml  or equivalent).

  • We need to create a "Default Shark Agent" template.
  • The Shark Teams' primary task during the workshop will be modifying this template's system prompts (e.g., turning the default agent into
  "Dogecoin Dan, the Chaos Lord") and using KiroCLI to deploy their custom agents to their AWS accounts.

  D. AWS Workshop Studio Provisioning (Medium Effort)
  We still need to provision the 15-20 AWS accounts. However, our IAM policies must now be updated to allow students to provision Bedrock Agents,
  create DynamoDB tables (if used), and use the Agentcore/Strands SDKs.

### Summary

  Doing this stateful makes it an incredibly premium, impressive workshop that perfectly mirrors real-world AI engineering. However, it requires our
  team to build a lot of the scaffolding (Chat UI, State-handling Lambda, KiroCLI templates) upfront. If we can hand them that solid foundation,
  they will have an amazing time focusing purely on the prompt engineering and the Shark Tank game.
