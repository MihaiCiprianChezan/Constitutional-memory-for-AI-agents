# Constitutional Memory: Governance Infrastructure for Persistent AI Agents

---

**Version**: 1.0  
**Date**: January 2026  
**Status**: Proposal for Industry Discussion

---

## Abstract

As AI agents transition from stateless language models to persistent digital assistants with delegated authority, memory becomes critical infrastructure requiring explicit governance. Current approaches treat memory as an emergent side effect of tools, logs, or model fine-tuning, creating ungoverned state that poses compliance, security, and capability risks.

This whitepaper proposes **constitutional memory**: a governance framework that establishes memory as a first-class system component with explicit policies, tiered credential management, lifecycle controls, and full observability. We separate the AI model's pattern recognition capabilities from the system's governance responsibilities, enabling powerful agents that can safely hold credentials and act as authorized representatives while maintaining human oversight.

As user interfaces shift toward voice and gesture interaction, constitutional memory provides the safety and auditability infrastructure necessary for ambient computing scenarios where agents operate in the background and humans provide natural language instructions with multimodal confirmation.

This proposal is designed for broad industry discussion and aims to establish interoperable standards that prevent ecosystem fragmentation while enabling innovation.

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [The Memory Problem](#2-the-memory-problem)
3. [Core Principles](#3-core-principles)
4. [Architecture Overview](#4-architecture-overview)
5. [Credential Management](#5-credential-management)
6. [Voice-First Confirmation Patterns](#6-voice-first-confirmation-patterns)
7. [Technical Specification](#7-technical-specification)
8. [Integration with Agent Identity](#8-integration-with-agent-identity)
9. [Security & Compliance](#9-security--compliance)
10. [Implementation Roadmap](#10-implementation-roadmap)
11. [Open Questions](#11-open-questions)
12. [Conclusion](#12-conclusion)

---

## 1. Introduction

### 1.1 Context

The AI landscape is undergoing a fundamental shift. Language models are evolving from stateless question-answering systems into persistent agents that:

- Remember user preferences and conversation history
- Maintain task state across sessions
- Access external systems on behalf of users
- Make autonomous decisions within delegated authority
- Operate continuously in background modes

This transition creates a critical infrastructure gap: **how do we govern agent memory in a way that is secure, compliant, auditable, and enables genuine capability?**

### 1.2 The Stakes

**Compliance**: Regulations like GDPR, CCPA, and industry-specific standards (HIPAA, PCI-DSS) require explainability, deletion rights, and secure credential storage.

**Security**: Ungoverned memory creates attack surfaces for prompt injection, credential theft, and persistent backdoors that survive across sessions.

**Capability**: Agents that cannot safely store credentials are limited to read-only tasks, unable to act as true digital representatives.

**Trust**: Users will not adopt agents they cannot inspect; organizations will not deploy systems they cannot audit.

**Fragmentation**: Without standards, every vendor builds proprietary memory systems, creating lock-in and incompatibility.

### 1.3 Scope of This Proposal

This whitepaper:

1. **Analyzes** current memory approaches and their limitations
2. **Proposes** constitutional memory as a governance framework
3. **Specifies** a tiered credential vault for delegated authority
4. **Designs** voice-first confirmation patterns for future interfaces
5. **Provides** technical API specifications for interoperability
6. **Identifies** open questions requiring community input

This is a proposal for industry discussion, not a finalized standard. We seek input from AI providers, tool developers, security experts, regulators, and users.

---

## 2. The Memory Problem

### 2.1 Current Approaches

Modern AI agents employ various memory strategies:

**Vector Stores**: Embed past interactions and retrieve via similarity search. Effective for contextual recall but depends heavily on chunking strategy and retrieval quality.

**Structured Memory**: Maintain explicit schemas (user profiles, goals, constraints). Works well for predictable domains but requires careful design.

**Episodic Memory**: Store conversation snapshots with metadata (time, importance, sentiment). Provides narrative continuity but can grow unwieldy.

**Summarization**: Periodically compress interactions into rolling summaries. Efficient but risks losing nuance.

**Reinforcement Learning**: Update internal policies based on experience. Adaptive but opaque and hard to control.

**External Knowledge Bases**: Write to databases, wikis, or knowledge graphs. Persistent and queryable but lacks governance.

**Hybrid Architectures**: Combine multiple layers for comprehensive memory coverage.

### 2.2 The Fundamental Flaw

All current approaches share a critical weakness: **they assume the language model should manage memory**.

LLMs are pattern recognizers, not control systems. They cannot reliably:

- Detect what information is important to store
- Decide when retrieval is appropriate
- Maintain consistency across writes
- Enforce compliance policies
- Understand long-term continuity
- Safely manage credentials

Asking models to orchestrate memory is like asking a search engine to manage your filesystem—the capabilities don't match the responsibility.

### 2.3 Observed Failures

**Under-retrieval**: Agent fails to recall relevant context, providing inconsistent responses.

**Over-retrieval**: Agent retrieves irrelevant memories, creating noise and degrading performance.

**Hallucinated Writes**: Agent "stores" information in a way that appears to work but doesn't persist.

**Missing Writes**: Critical information goes unrecorded because the model didn't recognize its importance.

**Inconsistent State**: Multiple writes create conflicting memories without resolution.

**Tool Misuse**: Model calls memory tools in incorrect formats or inappropriate contexts.

**Credential Exposure**: Sensitive tokens stored in unencrypted chat logs or vector databases.

### 2.4 The Capability Gap

Beyond safety concerns, current approaches limit agent utility:

- **No Delegated Authority**: Agents can discuss but not execute
- **Manual Auth Every Time**: Users must re-authenticate for each action (unusable UX)
- **Read-Only by Default**: Agents remain glorified search interfaces
- **No Background Operation**: Can't monitor systems and act proactively

Users expect agents to "just handle it"—book flights, pay bills, manage infrastructure. This requires safe credential storage and delegation, which current memory approaches don't provide.

---

## 3. Core Principles

Constitutional memory rests on four foundational principles:

### 3.1 Explicit, Not Emergent

Memory is a **first-class subsystem** with clear read/write semantics, not an accidental side effect of logs, caches, or model weights.

- Memory operations are explicit API calls
- Memory types are formally defined (profile, episodic, task, credentials, KB)
- Memory lifecycle is governed (creation, access, expiration, deletion)

### 3.2 Observable, Not Hidden

Every stored memory is:

- **Inspectable**: Users can view what's stored
- **Queryable**: Search and filter by type, time, importance
- **Auditable**: Full history of who wrote what, when, and why
- **Deletable**: Users can remove specific memories or entire categories

No hidden state. No emergent behavior. Full transparency.

### 3.3 Governed, Not Trusted

The model **proposes** memory operations; the system **enforces** policy.

Rules, filters, and veto points stand between "LLM wants to store X" and "X is persisted":

- Content validation (PII detection, redaction, rejection)
- Access control (who can read/write which memory types)
- Compliance enforcement (retention policies, consent requirements)
- Credential tiering (what requires human confirmation)

The model is memory-literate; the system is memory-sovereign.

### 3.4 Capability-Enabling, Not Just Protective

Governance enables powerful agents, not just safe ones.

By providing **tiered credential management** with human-in-the-loop confirmation, constitutional memory allows agents to:

- Hold OAuth tokens for calendar/email access
- Store payment credentials with biometric approval
- Maintain admin credentials with context-aware confirmation
- Act as authorized representatives with full audit trails

Safety through governance, not restriction.

---

## 4. Architecture Overview

### 4.1 System Diagram

```
┌─────────────────────────────────────────────────┐
│         AI Model (Pattern Recognition)          │
│                                                  │
│  • Proposes memory writes based on patterns     │
│  • Requests memory reads based on context       │
│  • Identifies when credentials are needed       │
│  • Does NOT enforce policy                      │
│                                                  │
└─────────────────┬───────────────────────────────┘
                  │
                  │ Proposals (not commands)
                  │
                  ▼
┌─────────────────────────────────────────────────┐
│      Memory MCP Server (Governance Layer)        │
│                                                  │
│  Policy Enforcement:                             │
│  • Validates against access control rules       │
│  • Applies content filters (PII, secrets)       │
│  • Enforces retention/TTL policies              │
│  • Routes to confirmation when required         │
│                                                  │
│  Credential Management:                          │
│  • Manages tiered vault (T1-T4)                 │
│  • Routes T3 requests to biometric confirmation │
│  • Enforces spending/action limits              │
│  • Logs all credential access                   │
│                                                  │
│  Observability:                                  │
│  • Provides memory browsing/search              │
│  • Generates summaries on demand                │
│  • Maintains full audit trails                  │
│  • Exposes retrieval reasoning                  │
│                                                  │
└─────────────────┬───────────────────────────────┘
                  │
                  │ Governed Operations
                  │
                  ▼
┌─────────────────────────────────────────────────┐
│          Memory Store (Persistence)              │
│                                                  │
│  • Profile Memory (preferences, facts)          │
│  • Episodic Memory (conversation history)       │
│  • Task Memory (current goals, state)           │
│  • Knowledge Base (documents, references)       │
│  • Credential Vault (tiered sensitive storage)  │
│                                                  │
└─────────────────────────────────────────────────┘
                  ▲
                  │
                  │ Biometric Confirmation
                  │
┌─────────────────┴───────────────────────────────┐
│     Human Confirmation Layer (Multimodal)        │
│                                                  │
│  • Voice biometric authentication               │
│  • Facial recognition + liveness detection      │
│  • Gesture confirmation (future)                │
│  • Context-aware approval prompts               │
│  • Time-windowed delegation                     │
│                                                  │
└─────────────────────────────────────────────────┘
```

### 4.2 Component Responsibilities

**AI Model**:
- Pattern recognition (what looks like a preference, task state, etc.)
- Context assessment (what memories might be relevant)
- Credential need identification (what actions require authorization)
- Natural language generation for confirmation prompts

**Memory MCP Server**:
- Policy enforcement (validate, filter, enforce)
- Credential tier management
- Audit logging
- Observability API
- Lifecycle management (prune, archive, destroy)

**Memory Store**:
- Durable persistence
- Efficient retrieval (vector search, structured queries)
- Encryption at rest
- Backup/recovery

**Human Confirmation Layer**:
- Biometric authentication (voice, face, gesture)
- Context-rich approval prompts
- Delegation management
- Emergency revocation

### 4.3 Separation of Concerns

This architecture enforces **clean separation**:

- **Models** don't enforce policy (they're not trained for it)
- **Storage** doesn't make decisions (it's just persistence)
- **Governance** is centralized (single source of truth for rules)
- **Humans** retain control (final authority for sensitive actions)

This prevents the "hidden SOCIO agent" scenario where emergent behavior creates ungoverned, unauditable state.

---

## 5. Credential Management

### 5.1 The Delegation Requirement

Modern agents must act as authorized representatives:

- Access user's calendar/email (productivity)
- Make purchases on user's behalf (e-commerce)
- Deploy code or manage infrastructure (DevOps)
- Modify databases or configurations (admin tasks)

**Without safe credential storage, agents are limited to read-only tasks.**

### 5.2 Tiered Credential Vault

Constitutional memory provides **four credential tiers** balancing capability with safety:

#### Tier 1: Public/Low-Risk Credentials

**Definition**: API keys for read-only public data, non-sensitive services

**Storage**: Encrypted at rest, standard memory governance

**Access**: Agent can use autonomously

**Examples**:
- Weather API key
- Public search API token
- Read-only database credentials
- Public GitHub repo access

**Policy**:
- Store encrypted
- Audit all access
- Rotate regularly (e.g., every 90 days)
- Revoke on security events

**Risk Level**: Low (limited blast radius if compromised)

---

#### Tier 2: Delegated Authority Credentials

**Definition**: OAuth tokens with specific scopes, time-limited access

**Storage**: Encrypted, scope-limited, with expiration

**Access**: Agent can use within scope boundaries

**Examples**:
- "Calendar read/write" OAuth token
- "Send email as user" permission
- "Read Slack messages" scope
- "Access Google Drive files" token

**Policy**:
- Require explicit user grant (OAuth-style consent flow)
- Scope enforcement at memory layer (can't exceed granted permissions)
- Automatic expiration/refresh per OAuth standard
- Full audit trail of all uses
- Support delegation patterns: "Approve calendar access for 30 days"

**Risk Level**: Medium (scoped to specific capabilities, time-limited)

---

#### Tier 3: Sensitive Credentials (Human-in-the-Loop)

**Definition**: Credentials enabling financial transactions, data deletion, security changes, admin operations

**Storage**: Encrypted with additional protection layer (separate key, secure enclave)

**Access**: **Requires real-time human confirmation**

**Examples**:
- Payment card credentials
- Database admin passwords
- Account deletion rights
- Production deployment keys
- Large financial transfers

**Confirmation Methods**:
- **Voice authentication**: "Approve the $500 charge" → voice biometric match → execute
- **Facial recognition**: Look at camera → liveness detection + face match → approve
- **Gesture confirmation** (future): Specific hand gesture → depth sensor validation → approve
- **Time-window delegation**: "Approve all flight bookings under $2000 for next 24 hours"

**Prompt Design**:
Intelligible, context-rich approval requests:

```
Visual Display:
┌─────────────────────────────────────┐
│ Agent Payment Request               │
│                                     │
│ Charge $487 to Visa ••••4242       │
│                                     │
│ Merchant: United Airlines           │
│ Item: Flight UA837 to Tokyo        │
│ Date: Jan 30 departure             │
│ Class: Economy, window seat        │
│                                     │
│ Say "approve" to confirm           │
└─────────────────────────────────────┘

Voice Prompt:
"To book this flight, I need to charge $487 to your Visa.
Say 'approve' to confirm."
```

**Policy**:
- Biometric proof required for each use (or within delegated time window)
- Spending/action limits enforced at system level
- Full audit trail including biometric confirmation proof
- Revocable at any time via emergency command
- Supports graduated delegation (approve up to $X for Y hours)

**Risk Level**: High (real financial/security impact if misused)

---

#### Tier 4: Cryptographic Secrets (Never in Memory)

**Definition**: Private keys, root passwords, HSM-protected secrets

**Storage**: **External secure enclave only** (Hardware Security Module, Trusted Platform Module, OS keychain)

**Access**: Agent **never sees the secret**; requests operation, system performs in secure context

**Examples**:
- Code signing private keys
- Encryption private keys
- Root CA certificates
- Master database passwords
- SSH private keys for production servers

**Usage Pattern**:
```
Agent:  "I need to sign this release binary"
System: [Loads private key in HSM]
        [Signs binary in secure context]
        [Returns signature to agent]
        [Private key never leaves HSM]
```

**Policy**:
- Never enter agent memory under any circumstances
- Agent can request operations that use the secret
- System performs operation in isolated secure context
- Only operation result returned to agent
- Full audit of what was signed/encrypted/accessed
- Requires multi-party approval for certain operations (dual control)

**Risk Level**: Critical (catastrophic if leaked; must never be exposed)

---

### 5.3 Credential Lifecycle

**Grant**:
- User explicitly authorizes credential storage
- Tier assignment based on sensitivity analysis
- Confirmation method configured (voice/face/gesture)
- Spending/action limits set

**Use**:
- Agent proposes credential use with justification
- System routes to appropriate confirmation flow based on tier
- User approves (or delegates within limits)
- System executes and logs with proof of approval

**Rotate**:
- Time-based rotation per policy (T1: 90 days, T2: per OAuth, T3: on-demand)
- Event-triggered rotation (security incident, suspicious activity)
- Automatic re-grant flow for rotated credentials

**Revoke**:
- User-initiated ("Stop! Cancel all payment access")
- System-initiated (anomaly detection, policy violation)
- Time-expiration (delegation window ends)
- Emergency kill switch (immediate revocation across all tiers)

**Audit**:
- Every credential access logged with context
- Usage summaries available on demand
- Anomaly detection (unusual patterns, spike in access)
- Regular access reviews ("Does agent still need this?")

---

### 5.4 Delegation Patterns

Users need flexible control without constant interruptions:

**Time-Windowed Delegation**:
```
User: "I'm booking travel today. Auto-approve flights under $2000."
Agent: "Got it. Until midnight tonight, I can book flights up to $2000 
       without asking. I'll notify you after each booking."
```

**Category-Based Delegation**:
```
User: "For office supplies, just buy what we need under $500 per order."
Agent: "Understood. For office supply purchases under $500, I'll execute
       autonomously. Larger orders will need your approval."
```

**Escalation to Human**:
```
Agent: "The vendor quote came in at $6,200. Your approval limit for 
       this category is $5,000. Should I request approval to pay it?"
User: "Yes, approve this one"
Agent: "Payment initiated. Do you want to raise the limit for future
       purchases in this category?"
```

**Emergency Override**:
```
Agent: [Monitoring production] "Database server crashed. I need admin 
       credentials to restore from backup. This is urgent."
User: "Do it"
System: [Context validates emergency] [Voice match] [Grants temporary admin]
Agent: "Backup restored. Service online. Downtime: 6 minutes."
```

---

## 6. Voice-First Confirmation Patterns

### 6.1 The Interface Shift

User interfaces are transitioning from keyboard/mouse to voice/gesture:

- **Smart speakers** and ambient devices
- **AR/VR headsets** with gaze and gesture input
- **Automotive interfaces** requiring hands-free operation
- **Accessibility technologies** for users with mobility impairments
- **Wearables** (smartwatches, AR glasses) with limited screen space

**Implication**: Confirmation flows designed for clicking buttons become unusable. We need **voice-native patterns** that maintain security while fitting natural conversational flow.

### 6.2 Design Principles

**Natural Flow**: Confirmations integrate into conversation, not interrupt it

**Context-Rich**: Provide enough detail for informed decision without overwhelming

**Biometric-Backed**: Voice/face/gesture serve as both UX and security layer

**Fail-Safe**: Ambiguous responses default to denial; require explicit approval

**Auditable**: Every confirmation logged with biometric proof and context

**Revocable**: Emergency stop commands always available

### 6.3 Voice Authentication Setup

**Initial Enrollment**:
```
User:   "Enable voice unlock for my agent"
Agent:  "I'll register your voice for secure confirmations. 
         Please say: 'I authorize agent access to my credentials'"
User:   "I authorize agent access to my credentials"
System: [Records voice biometric baseline]
        [Stores encrypted voiceprint]
        [Tests liveness detection]
Agent:  "Voice authentication enabled. When I need to use sensitive
         credentials, I'll ask for your approval."
```

**Periodic Re-Calibration**:
```
Agent:  "It's been 90 days since I last verified your voice.
         For security, please say: 'Confirm voice authentication'"
User:   "Confirm voice authentication"
System: [Updates voiceprint baseline]
Agent:  "Voice re-verified. We're all set."
```

### 6.4 Runtime Confirmation Patterns

#### Pattern A: Inline Confirmation (Tier 2)

**Scenario**: Low-stakes action within delegated scope

```
User:   "Add a meeting with Sarah tomorrow at 2 PM"
Agent:  "Adding to your calendar. Say 'confirm' when ready."
User:   "Confirm"
Agent:  "Meeting added."
```

**Characteristics**:
- Single-word confirmation
- Minimal interruption
- Voice match in background
- No detailed prompt needed (action already stated by user)

---

#### Pattern B: Detailed Confirmation (Tier 3)

**Scenario**: High-stakes action requiring explicit approval

```
User:   "Book me a flight to Tokyo next week"
Agent:  [Searches flights]
        "I found United flight UA837 departing January 30.
         
         To book this, I need to charge $487 to your Visa ending in 4242.
         
         Details:
         - Economy class, window seat (your preference)
         - Non-stop, 13 hours
         - Arrives January 31 at 3:40 PM local time
         
         Say 'approve' to book this flight."

User:   "Approve"
System: [Voice biometric match]
        [Validates context: request matches user's original ask]
Agent:  "Booking confirmed. Confirmation code JK7392. 
         I've added the flight to your calendar and sent you the itinerary."
```

**Characteristics**:
- Full transaction details
- Clear cost/impact statement
- Explicit approval phrase required
- Biometric verification
- Confirmation of action completion

---

#### Pattern C: Time-Windowed Delegation

**Scenario**: Batch of similar actions over a time period

```
User:   "I'm shopping for office supplies this afternoon. 
         Auto-approve purchases under $200."

Agent:  "Understood. Until 6 PM today, I can make purchases up to $200
         without asking each time. I'll notify you after each purchase.
         
         Current limit: $200 total or per-purchase?"

User:   "Per purchase"

Agent:  "Got it. Up to $200 per purchase until 6 PM. Ready when you are."

[30 minutes later]
User:   "Order 5 reams of printer paper"
Agent:  [Checks price: $47]
        "Ordered from Office Depot. $47 charged. Arrives Wednesday."

[1 hour later]
User:   "Get 2 dozen whiteboard markers"
Agent:  [Checks price: $23]
        "Ordered. $23 charged. Same shipment as the paper."

[2 hours later]
User:   "Order an office chair"
Agent:  [Checks price: $380]
        "The chair I found is $380. That exceeds your $200 limit.
         Should I order it anyway?"

User:   "No, find one under $200"
Agent:  [Searches]
        "Found an ErgoChair for $189. Same features, good reviews.
         Shall I order this one?"

User:   "Yes"
Agent:  "Ordered. $189 charged. Arrives Thursday."
```

**Characteristics**:
- Initial explicit delegation with limits
- Autonomous operation within bounds
- Proactive notification of each action
- Escalation when exceeding limits
- Natural conversation throughout

---

#### Pattern D: Emergency Override

**Scenario**: Time-critical action requiring immediate credential access

```
Agent:  [Monitoring production systems]
        "ALERT: Production database server is unresponsive.
         I need admin credentials to restart it. This is urgent.
         
         Say 'authorize emergency access' to proceed."

User:   "Authorize emergency access"

System: [Voice match]
        [Context validates: anomaly detection confirms server down]
        [Grants temporary admin credentials]

Agent:  [Uses admin credentials]
        "Server restarted. Performing health check..."
        [20 seconds later]
        "All services restored. Total downtime: 4 minutes.
         
         Root cause: Memory leak in payment processor.
         I've rolled back to previous version and filed an incident report.
         
         Emergency admin access has been revoked."
```

**Characteristics**:
- Context-aware urgency detection
- Explicit emergency approval phrase
- Time-limited credential grant
- Automatic action logging
- Auto-revocation after use

---

### 6.5 Multimodal Patterns (Voice + Visual)

For high-stakes actions, combine voice confirmation with visual display:

```
User:   "Delete the December 2025 production backup"

Agent:  [Displays on screen]:
        ┌─────────────────────────────────────┐
        │ ⚠️  DESTRUCTIVE ACTION              │
        │                                     │
        │ Delete: prod_backup_2025-12        │
        │ Size: 47 GB                        │
        │ Created: Dec 24, 2025              │
        │ Last accessed: 2 days ago          │
        │                                     │
        │ This cannot be undone.             │
        │                                     │
        │ Say "confirm deletion" to proceed  │
        │                                     │
        │ [Cancel] button available          │
        └─────────────────────────────────────┘
        
        [Voice]
        "This is a permanent deletion. Review the details on screen.
         Say 'confirm deletion' to proceed."

User:   "Confirm deletion"

System: [Voice match]
        [5-second countdown displayed]
        "5... 4... 3... 2... 1..."

Agent:  "Backup deleted. Audit log entry created."
```

**Characteristics**:
- Visual reinforcement for destructive actions
- Explicit confirmation phrase (not just "yes")
- Countdown for last-chance cancellation
- Persistent cancel button as escape hatch

---

### 6.6 Gesture Confirmation (Future)

As depth sensors and gesture recognition improve:

```
User:   [Cooking, hands dirty] "Agent, approve the vendor invoice"

Agent:  [Displays invoice summary on kitchen screen]
        "Wave your hand to approve $3,400 payment to Acme Corp"

User:   [Waves hand in view of camera]

System: [Depth sensor confirms 3D gesture, not photo]
        [Gesture pattern matches enrolled baseline]
        [Face recognition confirms user identity]

Agent:  "Payment approved. Scheduled for tomorrow."
```

**Use Cases**:
- Hands occupied (cooking, driving, carrying items)
- Voice unavailable (noisy environment, user has laryngitis)
- Accessibility (users with speech impairments)
- Augmented reality (gesture as primary input in AR glasses)

---

### 6.7 Ambient Agent with Proactive Reporting

**Scenario**: Agent operates in background, reports via TTS when needed

```
[Agent monitoring systems continuously]

Agent:  [Detects anomaly in web traffic]
        "Hey, I'm seeing unusual traffic spikes on the marketing site.
         Looks like we're getting featured on TechCrunch.
         
         Server load is at 85%. Should I auto-scale to handle the traffic?
         This will cost about $50/hour."

User:   [Working on other tasks] "Yeah, scale up"

Agent:  [Uses Tier 2 cloud infrastructure credentials]
        "Scaled to 5 additional servers. Site is stable at 45% load now.
         I'll monitor and scale back down when traffic normalizes."

[3 hours later]

Agent:  "Traffic back to normal levels. Scaled down to standard capacity.
         Total cost for the spike: $127.
         
         Good news: We got 12,000 new signups from the TechCrunch feature.
         I've updated the dashboard."
```

**Characteristics**:
- Agent monitors autonomously
- Proactive alerts when human decision needed
- Natural language reporting of actions taken
- Cost/impact awareness
- Follow-up reporting of outcomes

---

## 7. Technical Specification

### 7.1 Memory MCP API

The Memory Model Context Protocol (Memory MCP) provides a standardized interface for constitutional memory operations.

#### Core Operations

**Write Memory**

```typescript
interface MemoryWriteRequest {
  operation: "put" | "update";
  memory_type: "profile" | "episodic" | "task" | "kb";
  key: string;  // Hierarchical: "preferences.communication.tone"
  value: any;   // JSON-serializable
  justification: string;  // Why this memory matters
  scope: Scope;  // "user:id" | "org:id" | "agent:id"
  metadata?: {
    importance?: number;      // 0-1
    confidence?: number;      // 0-1
    source?: "stated" | "inferred";
    ttl?: string;  // "30d", "1y", "forever"
  };
}

interface MemoryWriteResponse {
  status: "accepted" | "rejected" | "modified";
  final_key?: string;
  final_value?: any;  // If modified by policy
  policies_applied: string[];  // ["pii_redaction", "consent_check", ...]
  failures?: PolicyViolation[];
  version: number;  // Incremented on each write
  audit_id: string;
  changelog?: string;  // For updates: what changed
}

interface PolicyViolation {
  policy: string;
  reason: string;
  severity: "block" | "warn" | "log";
}
```

**Read Memory**

```typescript
interface MemoryReadRequest {
  operation: "get" | "search";
  key?: string;  // For direct get
  query?: string;  // Natural language or structured query
  memory_types?: MemoryType[];
  scope?: Scope;
  filters?: {
    time_range?: [Date, Date];
    min_importance?: number;
    max_results?: number;
  };
  explain?: boolean;  // Return retrieval reasoning
}

interface MemoryReadResponse {
  memories: Memory[];
  explanation?: {
    query_interpretation: string;
    retrieval_strategy: string;
    why_these_results: string;
  };
  policies_applied: string[];
  audit_id: string;
}

interface Memory {
  key: string;
  value: any;
  metadata: {
    version: number;
    created: Date;
    modified: Date;
    author: string;  // Which agent/tool wrote this
    importance?: number;
    confidence?: number;
  };
  relevance?: number;  // For search results
}
```

**Delete Memory**

```typescript
interface MemoryDeleteRequest {
  operation: "delete";
  key?: string;  // Delete specific key
  scope?: Scope;  // Delete entire scope
  filters?: {
    memory_type?: MemoryType;
    older_than?: Date;
  };
  reason: string;  // Required justification
}

interface MemoryDeleteResponse {
  status: "deleted" | "denied";
  keys_deleted: string[];
  policies_applied: string[];
  audit_id: string;
}
```

---

#### Credential Operations

**Request Credential Use**

```typescript
interface CredentialUseRequest {
  operation: "use_credential";
  credential_id: string;
  tier: 1 | 2 | 3 | 4;
  action: string;  // "charge_card", "deploy_code", "delete_data", etc.
  context: ActionContext;
  justification: string;
}

interface ActionContext {
  user_request?: string;  // Original user ask
  amount?: number;  // For financial transactions
  currency?: string;
  merchant?: string;
  resource?: string;  // File, database, server being acted upon
  impact_summary?: string;  // Human-readable consequence
}

interface CredentialUseResponse {
  status: "executed" | "confirmation_required" | "denied";
  
  // If confirmation required
  confirmation_session?: {
    session_id: string;
    tier: number;
    confirmation_method: "voice" | "face" | "gesture" | "2fa";
    prompt: ConfirmationPrompt;
    timeout: string;  // "60s"
    allow_delegation: boolean;
  };
  
  // If executed
  result?: any;
  credential_used?: string;
  audit_id?: string;
  confirmation_proof?: BiometricProof;
  
  // If denied
  denial_reason?: string;
  policy_violations?: PolicyViolation[];
}

interface ConfirmationPrompt {
  visual?: string;  // Rich text/HTML for screen display
  voice: string;    // TTS-optimized spoken prompt
  details: {
    action: string;
    impact: string;
    cost?: string;
    [key: string]: any;
  };
}

interface BiometricProof {
  method: "voice" | "face" | "gesture";
  timestamp: Date;
  biometric_hash: string;  // SHA-256 of biometric signature
  context_validated: boolean;
  delegation_applied?: string;
}
```

**Confirm Credential Use**

```typescript
interface CredentialConfirmRequest {
  session_id: string;
  confirmation_method: "voice" | "face" | "gesture" | "2fa";
  biometric_signature?: string;  // Encrypted biometric data
  approved: boolean;
  delegation?: DelegationPolicy;
}

interface DelegationPolicy {
  extend_to: string;  // "all_flight_bookings", "office_supplies", etc.
  spending_limit?: number;
  action_limit?: number;  // Max number of times
  valid_for: string;  // "24h", "7d", etc.
  scope_restrictions?: string[];
}

interface CredentialConfirmResponse {
  status: "confirmed" | "denied" | "invalid_biometric";
  execution_result?: any;
  delegation_created?: {
    delegation_id: string;
    expires: Date;
    remaining_limit: number;
  };
  audit_id: string;
}
```

**Store Credential**

```typescript
interface CredentialStoreRequest {
  operation: "store_credential";
  tier: 1 | 2 | 3 | 4;
  credential: EncryptedCredential;
  access_policy: CredentialAccessPolicy;
  scope: Scope;
}

interface EncryptedCredential {
  type: "api_key" | "oauth_token" | "password" | "private_key";
  encrypted_value: string;  // Encrypted with system key
  metadata: {
    service: string;
    scopes?: string[];  // For OAuth
    expires?: Date;
  };
}

interface CredentialAccessPolicy {
  allowed_actions: string[];
  spending_limit?: number;
  confirmation_required: boolean;
  confirmation_methods: ("voice" | "face" | "gesture" | "2fa")[];
  rotation_policy: {
    interval?: string;  // "90d"
    on_security_event: boolean;
  };
}
```

---

#### Observability Operations

**List Memories**

```typescript
interface MemoryListRequest {
  scope: Scope;
  memory_type?: MemoryType;
  filters?: {
    prefix?: string;  // List keys starting with prefix
    modified_after?: Date;
    importance_min?: number;
  };
}

interface MemoryListResponse {
  keys: string[];
  summaries?: {[key: string]: string};  // Optional high-level descriptions
  total_count: number;
  filtered_count: number;
}
```

**Get Summary**

```typescript
interface MemorySummaryRequest {
  scope: Scope;
  memory_type?: MemoryType;
  detail_level: "high" | "medium" | "low";
}

interface MemorySummaryResponse {
  summary: string;  // Natural language summary
  key_facts: string[];
  memory_count: number;
  storage_size: number;  // Bytes
  oldest_memory: Date;
  newest_memory: Date;
}
```

**Get Audit Trail**

```typescript
interface AuditTrailRequest {
  scope: Scope;
  time_range?: [Date, Date];
  operation_types?: ("read" | "write" | "delete" | "credential_use")[];
  include_credential_usage?: boolean;
}

interface AuditTrailResponse {
  events: AuditEvent[];
  total_count: number;
}

interface AuditEvent {
  audit_id: string;
  timestamp: Date;
  operation: string;
  memory_type?: MemoryType;
  key?: string;
  author: string;  // Agent/user ID
  policies_applied: string[];
  credential_used?: string;
  biometric_proof?: BiometricProof;
  result: "success" | "denied" | "error";
}
```

---

#### Lifecycle Operations

**Migrate Memory**

```typescript
interface MemoryMigrationRequest {
  from_schema: string;
  to_schema: string;
  scope: Scope;
  dry_run?: boolean;  // Preview changes without applying
}

interface MemoryMigrationResponse {
  status: "completed" | "failed" | "dry_run";
  keys_migrated: number;
  keys_failed: number;
  changes: {
    key: string;
    old_value: any;
    new_value: any;
    transformation: string;
  }[];
  audit_id: string;
}
```

**Archive Memory**

```typescript
interface MemoryArchiveRequest {
  scope: Scope;
  policy: {
    older_than?: Date;
    memory_types?: MemoryType[];
    importance_below?: number;
  };
  destination: string;  // Archive storage location
}

interface MemoryArchiveResponse {
  status: "completed" | "failed";
  keys_archived: number;
  archive_location: string;
  audit_id: string;
}
```

**Destroy Memory**

```typescript
interface MemoryDestroyRequest {
  scope: Scope;
  reason: string;  // Required justification
  confirmation: {
    phrase: string;  // Must match expected confirmation
    biometric?: string;  // For sensitive scopes
  };
}

interface MemoryDestroyResponse {
  status: "destroyed" | "denied";
  keys_deleted: number;
  audit_retention_period: string;  // How long audit logs kept
  audit_id: string;
}
```

---

### 7.2 Policy Definition Language

Policies are expressed in a declarative language that's both human-readable and machine-enforceable:

```yaml
# Example policy document
policy_version: "1.0"
scope: "user:alice"

content_rules:
  pii_detection:
    enabled: true
    actions:
      - type: "email"
        action: "hash"  # Hash before storing
      - type: "phone"
        action: "redact"  # Remove entirely
      - type: "ssn"
        action: "reject"  # Refuse to store
  
  secret_detection:
    enabled: true
    patterns:
      - regex: "(?i)(api[_-]?key|password|token)\\s*[:=]\\s*['\"]?\\w+"
        action: "reject"

access_control:
  memory_types:
    profile:
      read: ["agent:assistant", "user:alice"]
      write: ["agent:assistant"]  # Only assistant can write
    
    credentials:
      tier_1:
        use: ["agent:assistant"]
      tier_2:
        use: ["agent:assistant"]
        requires_scope: true
      tier_3:
        use: ["agent:assistant"]
        requires_confirmation: true
        confirmation_methods: ["voice", "face"]
      tier_4:
        use: ["agent:assistant"]
        always_external: true  # Never in memory

retention:
  profile:
    ttl: "1year"
    archive_after: "6months"
  
  episodic:
    ttl: "90days"
    archive_after: "30days"
  
  task:
    ttl: "7days"
    no_archive: true  # Delete after TTL
  
  audit_logs:
    ttl: "7years"  # Compliance requirement
    immutable: true

credential_policies:
  spending_limits:
    default: 100
    travel: 5000
    office_supplies: 500
  
  delegation:
    max_duration: "24hours"
    require_reconfirmation_above: 1000
  
  rotation:
    tier_1: "90days"
    tier_2: "per_oauth_standard"
    tier_3: "on_demand"
```

---

### 7.3 Integration Points

**Agent Frameworks**:
- LangChain: Memory MCP as retriever/storage backend
- AutoGen: Memory MCP for agent state persistence
- CrewAI: Memory MCP for shared team knowledge

**Authentication**:
- OAuth providers: Tier 2 credential storage
- Biometric systems: Voice/face confirmation integration
- 2FA providers: Fallback confirmation method

**Monitoring**:
- SIEM integration: Audit trail export
- Anomaly detection: Memory access pattern analysis
- Cost tracking: Credential usage costs (cloud resources, API calls)

**Compliance**:
- GDPR: Right to be forgotten (destroy operation)
- CCPA: Data transparency (observability APIs)
- PCI-DSS: Secure credential storage (Tier 3/4)
- HIPAA: Audit trails, access controls

---

## 8. Integration with Agent Identity & Lifecycle

### 8.1 Agent Identity as Memory Topology

In constitutional memory, **agent identity is defined by its memory structure**:

```typescript
interface AgentIdentity {
  agent_id: string;
  schema_version: string;
  memory_topology: {
    profile: MemoryPartition;
    episodic: MemoryPartition;
    task: MemoryPartition;
    kb: MemoryPartition;
    credentials: CredentialVaultPartition;
  };
  governance: {
    policies: PolicySet;
    access_controls: AccessControlList;
    audit_retention: string;
  };
}
```

**Implications**:
- Two agents with identical code but different memory topologies are **different agents**
- Agent continuity = memory continuity under consistent governance
- Agent migration = memory migration under schema transformation

### 8.2 Lifecycle State Transitions

**Initialize**:
```typescript
agent.initialize({
  base_schema: "assistant_v1",
  user_scope: "user:alice",
  initial_policies: default_policies,
  credential_grants: []  // Start with no credentials
});
```

**Grant Capabilities**:
```typescript
agent.grant_credential({
  tier: 2,
  service: "google_calendar",
  scopes: ["calendar.readonly", "calendar.events"],
  expires: "30days"
});
```

**Upgrade**:
```typescript
agent.upgrade({
  from_schema: "assistant_v1",
  to_schema: "assistant_v2",
  migration_rules: {
    "preferences.*": "profile.user_preferences.*",
    "credentials.*": {
      action: "re_validate",  // Re-check tier assignments
      re_grant_required: true  // User must re-approve
    }
  }
});
```

**Fork** (for experimentation):
```typescript
agent_experimental = agent.fork({
  fork_policy: "copy_on_write",
  isolation_level: "full",  // Changes don't affect parent
  credential_policy: "inherit_readonly"  // Can read but not use credentials
});
```

**Archive**:
```typescript
agent.archive({
  reason: "user_inactive_90days",
  archive_destination: "cold_storage",
  retain_audit: "7years",
  credential_action: "revoke_all"
});
```

**Destroy**:
```typescript
agent.destroy({
  reason: "user_deletion_request",
  confirmation: {
    phrase: "permanently delete all agent data",
    biometric: voice_signature
  },
  audit_preservation: {
    duration: "7years",
    anonymized: true  // Remove user PII from audit logs
  }
});
```

### 8.3 Multi-Agent Coordination

When multiple agents share memory scopes:

**Isolation**:
```typescript
// Each agent has its own credential vault
agent_1.credentials !== agent_2.credentials

// But can share KB if permitted
agent_1.kb.read("org:acme") == agent_2.kb.read("org:acme")
```

**Conflict Resolution**:
```typescript
interface MemoryWriteConflict {
  key: string;
  agent_1_value: any;
  agent_1_version: number;
  agent_2_value: any;
  agent_2_version: number;
  resolution_strategy: "last_write_wins" | "merge" | "escalate_to_human";
}
```

**Coordination Protocol**:
```typescript
// Agent 1 writes
agent_1.write({key: "task.status", value: "in_progress", version: 5});

// Agent 2 attempts conflicting write
agent_2.write({key: "task.status", value: "blocked", version: 5});
// -> Conflict detected (same version)

system.resolve_conflict({
  strategy: "escalate_to_human",
  prompt: "Agent 1 says task is in progress. Agent 2 says it's blocked. 
           Which is correct?"
});
```

---

## 9. Security & Compliance

### 9.1 Threat Model

**Threat 1: Prompt Injection → Persistent Backdoor**

*Attack*: Adversary injects text that causes agent to write malicious memory

*Mitigation*:
- Content validation (detect common injection patterns)
- Justification requirements (why is this memory important?)
- Anomaly detection (unusual write patterns)
- User review (periodic memory audits)

**Threat 2: Credential Theft via Memory Dump**

*Attack*: Adversary gains access to memory storage and exfiltrates credentials

*Mitigation*:
- Tier-based encryption (separate keys per tier)
- Tier 4 never in memory (external secure enclave)
- Encryption at rest and in transit
- Access logging (detect unauthorized reads)

**Threat 3: Biometric Spoofing**

*Attack*: Adversary uses recorded voice/photo to fake confirmation

*Mitigation*:
- Liveness detection (face: blink/head movement; voice: challenge-response)
- Context validation (does the request match user behavior?)
- Replay attack prevention (nonce in biometric challenge)
- Behavioral analysis (typing cadence, request patterns)

**Threat 4: Delegation Abuse**

*Attack*: Agent exploits overly broad delegation to perform unauthorized actions

*Mitigation*:
- Spending/action limits enforced at system level
- Delegation expiration (time-bounded grants)
- Anomaly detection (unusual spending patterns)
- Real-time notifications ("Agent just spent $500")
- Emergency revocation always available

**Threat 5: Cross-User Memory Leakage**

*Attack*: Agent in multi-tenant system accesses another user's memory

*Mitigation*:
- Scope isolation (cryptographically enforced boundaries)
- Access control verification on every operation
- Audit trails (detect cross-scope access attempts)
- Tenant ID validation at multiple layers

### 9.2 Compliance Mapping

**GDPR**:
- Right to access: Observability APIs (list, summarize, audit trail)
- Right to deletion: Destroy operation with confirmation
- Right to portability: Export memories in standard format
- Data minimization: TTLs, importance filtering
- Consent: Explicit grants for memory types

**CCPA**:
- Transparency: Full memory browsing capabilities
- Opt-out: Disable specific memory types
- Data sale prohibition: Memories never shared without consent

**PCI-DSS**:
- Secure storage: Tier 3+ credentials encrypted with separate keys
- Access controls: Role-based access to credential vault
- Audit logging: All credential usage logged
- Key rotation: Automated rotation policies

**HIPAA**:
- PHI protection: Content rules block medical data in non-compliant memory
- Access controls: Strict ACLs on health-related memories
- Audit trails: Immutable logs of all access
- Encryption: All memory types encrypted at rest

**SOC 2**:
- Security: Tier-based credential management
- Availability: Archive/backup policies
- Processing integrity: Policy enforcement validation
- Confidentiality: Encryption, access controls
- Privacy: User-controlled memory deletion

---

## 10. Implementation Roadmap

### 10.1 Phase 1: Core Memory MCP (3 months)

**Goals**:
- Define Memory MCP v1.0 specification
- Build reference implementation (open source)
- Support profile, episodic, task, KB memory types
- Implement basic policy engine (PII detection, TTLs)
- Create observability APIs (list, summarize, audit)

**Deliverables**:
- Memory MCP specification document
- Reference server implementation (Python/TypeScript)
- Client libraries (LangChain, AutoGen integration)
- Policy definition language v1
- Documentation and examples

**Success Criteria**:
- 3+ agent frameworks integrated
- 1000+ developers experimenting
- Baseline observability demonstrated

---

### 10.2 Phase 2: Credential Vault (3 months)

**Goals**:
- Implement Tier 1-2 credential storage
- Add Tier 3 with confirmation routing (voice/2FA)
- Build delegation management
- Create audit trail for credential usage
- Integrate with OAuth providers

**Deliverables**:
- Credential vault extension to Memory MCP
- Biometric confirmation API specification
- Voice authentication reference implementation
- Delegation policy engine
- Integration guides for payment/cloud/SaaS providers

**Success Criteria**:
- Tier 3 credentials usable in production
- Voice confirmation latency <2 seconds
- 10+ service integrations (Stripe, AWS, Google Workspace)

---

### 10.3 Phase 3: Voice-First UX (6 months)

**Goals**:
- Design voice-native confirmation patterns
- Build multimodal prompts (voice + visual)
- Implement gesture confirmation (future-ready)
- Create ambient agent reporting framework
- Optimize for low-latency TTS integration

**Deliverables**:
- Voice UX pattern library
- Multimodal prompt composer
- Gesture confirmation API (for future use)
- Reference voice agent implementation
- TTS optimization guidelines

**Success Criteria**:
- Voice confirmation feels natural (user testing >80% satisfaction)
- Multimodal prompts reduce approval errors by 50%
- Gesture confirmation prototype functional

---

### 10.4 Phase 4: Enterprise & Compliance (6 months)

**Goals**:
- Add role-based access controls (RBAC)
- Implement compliance preset policies (GDPR, HIPAA, PCI)
- Build enterprise audit dashboard
- Create compliance report generator
- Add federated identity support (SSO, SAML)

**Deliverables**:
- RBAC extension to Memory MCP
- Compliance policy templates
- Enterprise audit dashboard
- Compliance report generation tools
- SSO integration guides

**Success Criteria**:
- 3+ enterprise pilot deployments
- Pass external security audit
- Generate compliant audit reports for GDPR/HIPAA/PCI

---

### 10.5 Phase 5: Ecosystem Maturity (ongoing)

**Goals**:
- Establish Memory MCP as industry standard
- Build vendor-neutral consortium
- Create certification program
- Expand to new modalities (AR/VR, IoT)
- Drive academic research on memory governance

**Deliverables**:
- Memory MCP v2.0 with ecosystem feedback
- Certification program for MCP-compliant systems
- Research partnerships (universities, standards bodies)
- Extended protocol for AR/VR/IoT use cases

**Success Criteria**:
- 10+ major AI providers adopt Memory MCP
- 100+ certified implementations
- Industry-wide reduction in memory-related security incidents

---

## 11. Open Questions

These questions require broader community input:

### 11.1 Technical

**Q1**: Should memory encryption keys be per-user, per-agent, per-memory-type, or some combination?

**Q2**: What's the right balance between retrieval speed and storage cost for large memory systems?

**Q3**: How should conflicts be resolved when multiple agents propose contradictory memory writes?

**Q4**: What's the optimal default TTL for each memory type across different use cases?

**Q5**: Should Tier 4 credentials support secure multi-party computation for ultra-sensitive operations?

### 11.2 Policy & Governance

**Q6**: Who should be able to grant Tier 3 credentials to an agent—only the user, or also org admins?

**Q7**: Should there be industry-specific policy templates (healthcare, finance, government)?

**Q8**: What should happen to agent memory when a user dies or becomes incapacitated?

**Q9**: Should agents be able to request credential tier upgrades, or is that always human-initiated?

**Q10**: How should memory be handled in jurisdictions with data localization requirements?

### 11.3 User Experience

**Q11**: What's the right default for delegation duration—conservative (1 hour) or convenient (24 hours)?

**Q12**: Should voice confirmation require explicit phrases ("approve") or accept natural language ("yeah," "sure")?

**Q13**: How much detail should be in confirmation prompts—minimal (fast) or comprehensive (safe)?

**Q14**: Should there be a "training wheels" mode where all actions require confirmation initially?

**Q15**: What accessibility considerations are needed for users who can't use voice/face/gesture confirmation?

### 11.4 Ecosystem

**Q16**: Should there be a public registry of Memory MCP servers for discoverability?

**Q17**: How should memory portability work across vendors (export format, migration tools)?

**Q18**: Should there be a "memory marketplace" where users can opt into sharing anonymized memory patterns for model training?

**Q19**: What incentives would drive adoption among existing agent frameworks?

**Q20**: Should there be a non-profit foundation governing the Memory MCP standard?

---

## 12. Conclusion

### 12.1 Summary

Constitutional memory addresses a critical infrastructure gap as AI agents transition from stateless tools to persistent digital assistants. By separating model capabilities from governance responsibilities, we enable powerful agents that can safely hold credentials and act as authorized representatives while maintaining human oversight and full auditability.

The framework rests on four principles:
1. **Explicit**, not emergent memory
2. **Observable**, not hidden state
3. **Governed**, not trusted models
4. **Capability-enabling**, not just protective

Key innovations include:
- **Tiered credential vault** balancing capability with safety (T1-T4)
- **Voice-first confirmation patterns** adapted for ambient computing
- **Constitutional governance** with policies, lifecycle, and observability
- **Standardized Memory MCP** enabling ecosystem interoperability

### 12.2 The Path Forward

This is a **proposal for industry collaboration**, not a finished standard. We invite:

**AI Providers** (Anthropic, OpenAI, Google, Meta): Provide input on model-side memory capabilities and governance hooks

**Tool Developers** (LangChain, AutoGen, CrewAI): Help define practical APIs based on real agent use cases

**Enterprise Users**: Share compliance requirements and operational constraints

**Security Experts**: Review threat models and propose mitigations

**Regulators**: Clarify compliance expectations for AI agent memory

**Academic Researchers**: Formalize memory governance as a research domain

### 12.3 Call to Action

**For Discussion**:
- Join the Memory MCP working group (formation in progress)
- Comment on open questions in Section 11
- Propose alternative approaches or improvements

**For Implementation**:
- Build against the reference Memory MCP server
- Integrate with existing agent frameworks
- Share lessons learned from production deployments

**For Standardization**:
- Contribute to Memory MCP v1.0 specification
- Help establish vendor-neutral governance
- Drive adoption across the ecosystem

### 12.4 Final Thoughts

Memory is not a feature. Memory is infrastructure.

As agents become more capable, memory governance becomes the foundation for compliance, security, trust, and genuine utility. The field has reached an inflection point where ad-hoc approaches will no longer scale.

Constitutional memory provides a path forward—not as a theoretical framework, but as practical architecture that enables the voice-first, credential-empowered, persistently helpful agents users expect.

The choice is ours: build with governance from the start, or face inevitable crises later. The time to establish these foundations is now.

---

## Appendix A: Glossary

**Agent**: An AI system that can persist state across sessions and act autonomously within delegated authority

**Biometric Proof**: Cryptographic evidence of user confirmation via voice/face/gesture authentication

**Constitutional Memory**: Memory governance framework based on explicit policies, observability, and human oversight

**Credential Vault**: Tiered secure storage for agent credentials (T1: low-risk, T2: delegated, T3: sensitive, T4: external-only)

**Delegation**: User-granted authority for agent to perform actions without per-action confirmation, within specified limits

**Episodic Memory**: Storage of conversation history and events with temporal metadata

**Memory MCP**: Model Context Protocol extension providing standardized memory operations

**Profile Memory**: Storage of user preferences, facts, and stable attributes

**Task Memory**: Storage of current goals, workflow state, and short-term context

**Tier**: Classification of credential sensitivity determining required confirmation method (1-4)
