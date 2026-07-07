# AI Shark Tank: Hybrid Architecture

Collaboration: Department of Cloud Infrastructure 🤝 Department of AI
By keeping API Gateway and AWS Lambda in the stack, we create a strict separation of concerns. The Cloud team builds the roads and security, while the AI team builds the brains and memory.

## The New Architecture Flow

1. User (S3 Frontend): Types a pitch and a session_id into the chat UI.
2. API Gateway (Cloud Infra): Receives the POST /pitch request. It handles CORS, rate limiting, and initial payload validation.
3. AWS Lambda (The Handshake): Triggered by API Gateway. This is where the two worlds meet. The Lambda handler parses the HTTP event and uses the Strands SDK to pass the payload to AgentCore.
4. AgentCore & Strands SDK (AI Dept): - AgentCore retrieves the chat history for that session_id.

- Strands SDK orchestrates the multi-shark personas and runs any required tools.
- Bedrock (Nova/Claude) processes the intelligence.

5. The Return: Strands SDK returns the structured JSON to the Lambda function -> Lambda formats the HTTP 200 response -> API Gateway delivers it to the browser.

## The "Startup Team" (Division of Labor)

Each participating team acts as a single "Startup" consisting of 4 members (2 DCCI + 2 DAI) sharing one AWS account. The team builds the complete infrastructure and AI together before entering the bracket progression competition.

### Cloud Engineers (DCCI Members)

- Focus: Networking, Serverless Compute, and Security.
- Tasks:
  - S3 Hosting: Deploy the static React/Vanilla JS frontend and configure bucket policies.
  - API Gateway: Set up the HTTP API. Configure CORS (a classic cloud learning moment) and set up throttling to prevent abuse.
  - IAM Least Privilege: Write the strict IAM Role for the Lambda function. It must explicitly allow `bedrock:InvokeModel` and DynamoDB access (if used for memory).
  - Lambda Deployment: Package the Python Lambda function *including* the Strands SDK dependencies (using Lambda Layers or ZIP packaging) and set up the API Gateway trigger.

### AI Engineers (DAI Members)

- Focus: Prompt Engineering, Tool Creation, and Agentic State.
- Tasks:
  - Strands SDK Agents: Define the Shark personas (e.g., Brutal VC, Chaos Lord) using Strands SDK decorators (`@agent`).
  - Tool Building: Write Python functions decorated with `@tool` that the Sharks can use (e.g., `calculate_runway()`, `search_competitors()`).
  - Agent Deployment: Deploy the stateful memory configuration and the agent framework.
  - JSON Contract: Ensure the LLM strictly adheres to the JSON schema required by the frontend.

## The Technical Handshake (Lambda Code)

- The AWS Lambda function becomes the bridge. The Cloud Infrastructure team provides the event object, and the AI team's Strands SDK code processes it.
- Here is pseudo-code showing what the Lambda function looks like in this hybrid model:

```python
import json
# AI Team imports their Strands SDK agent
from shark_agent import run_shark_tank_agent 

def lambda_handler(event, context):
    # 1. CLOUD TEAM: Parse the incoming API Gateway event
    body = json.loads(event.get('body', '{}'))
    user_pitch = body.get('pitch_text')
    session_id = body.get('session_id') # Crucial for AgentCore state
    
    try:
        # 2. AI TEAM: Pass to Strands SDK / AgentCore
        # The SDK handles memory retrieval, Bedrock invocation, and tool use automatically
        agent_response = run_shark_tank_agent(
            session_id=session_id,
            user_input=user_pitch
        )
        
        # 3. CLOUD TEAM: Format the HTTP response for API Gateway
        return {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            'body': json.dumps(agent_response) # The structured JSON
        }
        
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }

```

## Summary of the Win-Win

Cloud Infra gets to teach: API Gateway routing, Lambda event structures, CORS, and IAM security.
AI gets to teach: Stateful memory management with AgentCore, ReAct loops, tool usage, and prompt engineering with Strands SDK.
The Students get: A highly realistic enterprise architecture where cloud infrastructure wraps AI logic.
