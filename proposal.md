# AWS STUDENT BUILDER GROUP PHLIIPPINES

## Event Name/Activity Name

PLANET INDIGO - The Sentinel Sphere
AWS Intergalactic AI Shark Tank (DCCI x DAI)

## Date, Time, and Duration

Date: 4nd Week of July, 2026
Time: TBA
Duration: TBA

## Type of Event

Type: Face-to-Face Collaborative Workshop - Hands-On Project Build
Venue - (TBA) - [ considering AWS office  ]
AWS Access Request
Access Required: YES

## Instructor Access

- Live demonstration of CloudWatch dashboard creation and alarm configuration
- Show Lambda function creation and deployment
- Demonstrate event-driven architecture with Lambda triggers
- Show CloudWatch Logs and metrics analysis

## Goals and Objectives

### Primary Goal: Collaborate across cloud infrastructure and AI tracks to design, deploy, and monitor a live, stateful AI-powered chat application featuring multi-turn negotiation using an event-driven serverless architecture and AI Agents.

#### Key Objectives

**DCCI Learning Objectives:**

- Master serverless app deployment using Amazon S3 Static Web Hosting and AWS Lambda.
- Configure cross-origin communications securely using Amazon API Gateway HTTP APIs.
- Enforce security best practices using IAM Least-Privilege Policies for Lambda.
- Implement system observability using Amazon CloudWatch Logs to trace live execution paths.

**DAI Learning Objectives:**

- Master multi-persona prompt engineering and agent orchestration using Amazon Bedrock Agents / Strands SDK.
- Manage conversation state and memory for multi-turn negotiations.
- Implement strict Structured JSON Output contracts so AI data strings reliably map to standard UI elements without crashing the browser.
- Program execution pipelines using KiroCLI, AWS CLI, and Bedrock SDKs.

## Speakers/Hosts

**Raphael Jonathan Flores - DCCI Lead**
Task: Architecture Patterns, Serverless decoupling theory, and CloudWatch Trace Observability.

**Roan Manansala - DCCI Co-Lead**
Tasks: Live deployment demo of API Gateway, S3 Bucket permissions, IAM configuration, and CloudWatch metrics.

**Lawrence Panes - DAI Lead**
Tasks: Prompt engineering patterns, model parameter configurations, and mapping the Bedrock SDK inside AWS Lambda.

## Expected number of Participants

- Expected Number of Participants: 30 - 50 members (Split into groups/pairs of 1-3 members per AWS Workshop Studio Account)
Profile:
- DCCI members with complete foundational knowledge (Network, Compute, Storage, Security)
- Some programming experience (Python or JavaScript helpful for Lambda)
- Ready for advanced cloud architecture patterns
- Preparing for Chrome (containers) and Capstone project

## Within the Department or Organization-wide event

- Department Event (DCCI Members Only)
- This session introduces advanced monitoring and serverless patterns crucial for the CloudVoyage capstone project.

## Project Specifications Matrix

Collaborating Units: Dept. of Cloud Computing & Infrastructure (DCCI) × Dept. of Artificial Intelligence (DAI)
Implementation Timeline: 4th Week of July, 2026
Event Modality: Face-to-Face Collaborative Workshop — Hands-On Project Build
Target Venue: TBA (Under active consideration: AWS Philippines Office)
Target Cohort: 30 - 50 DCCI & DAI Members

## Architectural Blueprint & Technical Division

- The system blueprint segregates duties between cloud infrastructure and AI engineering, simulating the functional workflow of an enterprise-grade technical squad.
**DCCI Track (Infrastructure & Observability)**
- Storage Edge: Provision an Amazon S3 bucket for global read-access, serving as the static hosting environment for the sentinel UI.
- Ingestion Layer: Establish synchronous Amazon API Gateway HTTP APIs (POST /pitch) integrated with strict CORS security protocols.
- Security Controls: Develop granular AWS IAM Least-Privilege Policies to confine Lambda execution to specific Bedrock resource scopes.
- Observability Network: Deploy persistent monitoring via Amazon CloudWatch Logs to audit execution traces and visualize system latencies.

**DAI Track (AI Systems Engineering)**
Model Orchestration: Program the stateful backend logic using Amazon Bedrock Agents and Strands SDK to handle multi-turn conversational transactions.
Multi-Persona Foundations: Design complex system instructions and agent templates via KiroCLI/Harness to facilitate concurrent, differentiated AI investor archetypes with memory.
Strict Payload Contracts: Mandate precise Structured JSON Output from the model to ensure reliable data mapping into frontend chat component structures.

**Event Game Mechanics & Competition Dynamic**
- Groups will choose or be assigned to be **The Sharks** (Judging Panels) or the **Entrepreneur Team**.
- Shark groups will engineer the AI agent backend. Entrepreneur groups will use the frontend to pitch and engage in multi-turn negotiation with the Sharks.
- **The Competition (Human Offense vs. AI Defense)**:
  - **Shark Teams** act as the defense. Their goal is to engineer rigorous, hard-to-impress AI investor personas that grill the entrepreneurs and defend against weak pitches. They are judged on the AI's intelligence and adherence to character.
  - **Entrepreneur Teams** act as the offense. They connect to a Shark team's AI and use human negotiation skills to convince the AI to output `"status": "FUNDED"`. They are ranked on a live leaderboard by the total imaginary capital they raise.

## Cloud Architecture

1. User (Entrepreneur Team)
2. Amazon S3 (Static Web Hosting)

- index.html (Chat UI), app.js, style.css, assets/
- S3 Bucket Policy for Public Read Access (Static Hosting)

3. Amazon API Gateway (HTTP API)
4. AWS Lambda (Stateful Session Handler)

- Processes incoming chat messages and maintains `session_id`.
- Interacts with Bedrock Agentcore / Strands SDK for memory management.
- Returns Structured JSON Response to Frontend (Contains ongoing multi-turn Shark responses).
- IAM Security for Lambda Execution Role (Least Privilege).

5. Amazon Bedrock Agents (AI Model & State Execution)

- Managed Bedrock Agents (or Strands SDK backend) handling memory and multi-turn ReAct loops.
- Amazon Nova (or Claude/Titan) for multi-persona prompt engineering.
- Three Shark Personas (Configured via KiroCLI).
- Evaluate Startup, ask follow-up questions, negotiate.
- Return Strict JSON.

Prompt Includes:

1. Brutal VC
2. Retail Expert
3. Chaos Lord
4. Strict JSON Schema

## API & Data Contract Definition

To avoid structural conflicts, both teams are constrained during the workshop by this exact single-turn communication protocol:

Interface Request Specification (POST /chat)
{
  "session_id": "uuid-1234-5678",
  "startup_name": "Stellar Wash",
  "user_message": "An automated laundry array using black holes for spin cycles. What do you think?"
}

Downstream Response Contract (200 OK Logic Mapping - Multi-Turn)
{
  "session_id": "uuid-1234-5678",
  "shark_responses": [
    {
      "name": "Chad Nebula (VC Bro)",
      "status": "NEGOTIATING",
      "quote": "Black holes? What's the containment cost on that? Tell me your customer acquisition cost."
    },
    {
      "name": "Lady Margins (Logistics)",
      "status": "NEGOTIATING",
      "quote": "I like the utility, but spin cycles are slow. Can you speed it up?"
    },
    {
      "name": "Dogecoin Dan (Chaos Lord)",
      "status": "FUNDED",
      "quote": "Vibes are celestial. Take 2 million in monopoly bills right now!"
    }
  ]
}
