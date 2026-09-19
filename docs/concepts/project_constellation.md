# Project Constellation

Status: exploratory architecture

Project Constellation is the umbrella for an end to end ecosystem that can take a human goal from incomplete intent through discovery, research, product and architecture reasoning, execution planning, parallel delivery, independent verification, adaptation, evidence capture, and final acceptance.

The project is deliberately larger than any individual agent, model, skill, workflow, or harness.

The central architectural idea is:

> Constellation owns intent, canonical state, evidence, and authority. Harnesses provide execution. Skills and plugins provide capabilities. Models provide reasoning. The Viewer projects the system back to the human.

This document is the central concept space for exploring that architecture. It is intentionally broader than any current implementation.

## 1. The problem

Current agent systems often collapse several different concerns into one harness or one conversation:

1. Understanding what the human actually wants.
2. Discovering missing requirements and constraints.
3. Researching unknowns.
4. Designing the product and architecture.
5. Turning that understanding into executable work.
6. Selecting tools, skills, models, and workflows.
7. Coordinating parallel workers.
8. Verifying that outputs satisfy the original goal.
9. Recording why decisions changed.
10. Giving the human meaningful visibility and control.

That works for small tasks. It becomes fragile for long running engineering work because context, authority, planning, execution, evidence, and orchestration become entangled.

Project Constellation separates those responsibilities while keeping them connected through shared contracts and a common graph.

## 2. System boundary

Project Constellation should sit above individual execution environments.

Examples of execution environments include Codex, OpenCode, Antigravity, Hermes, OpenClaw, and future harnesses.

Constellation should not require one of them to be the permanent centre of the system.

A harness may disappear, change its API, gain new capabilities, or be replaced entirely. The project goal, decisions, evidence, WorkSets, and execution history should survive.

The intended shape is:

```text
Human
  ↓
North Star
  ↓
Waystar
  ↓
Capabilities and Research
  ↓
WorkSet Compiler
  ↓
WorkSet Graph
  ↓
Constellation Runtime
  ↓
Harness Adapters
  ↓
Workers
  ↓
Verification and Acceptance
  ↓
Evidence and Chronicle

Viewer projects every stage back to the human.
```

## 3. Architectural principles

### 3.1 Canonical state lives outside agents

Agents reason about state. They do not own canonical state.

Important project state should be explicit, versioned, inspectable, and replayable.

### 3.2 Capabilities are more important than implementations

Constellation should ask for a capability such as specification review, repository inspection, deep research, verification, or code generation.

A particular skill, plugin, model, or harness may satisfy that capability.

The core should not depend on a specific provider unless the contract truly requires it.

### 3.3 Evidence is stronger than claims

A worker saying that work is complete is not proof.

Completion should be based on declared acceptance contracts and independent evidence.

### 3.4 Context should be projected, not copied

Workers should receive the minimum sufficient context for their responsibility.

The whole project should not be repeatedly injected into every model call.

### 3.5 Adaptation is allowed but governed

The system should adapt when evidence invalidates the current plan.

Adaptation should have explicit authority boundaries. A worker should not silently redefine the product because it encountered a difficult implementation detail.

### 3.6 Human authority is explicit

Some changes can be automatic. Some require review. Some must require human approval.

Authority should be represented as data and policy rather than implied by which model is currently running.

### 3.7 Work is a graph

Dependencies, evidence, decisions, requirements, and execution should be modelled as connected objects.

Methodology should emerge from the dependency structure rather than being imposed globally.

### 3.8 Workflows are composable

There should not be one mandatory Constellation process.

A workflow can be small for a simple bug and much richer for a new regulated platform.

### 3.9 Harnesses are replaceable

The runtime owns coordination.

Harnesses execute bounded work through adapters.

### 3.10 Scale through bounded scopes

Large swarms should be built from bounded subgraphs and management loops, not one enormous shared conversation.

## 4. North Star

North Star is the durable statement of why the project exists and what success means.

It is more than an initial prompt.

A North Star may contain:

* Goals
* Success criteria
* Constraints
* Non goals
* Risk tolerance
* Budgets
* Required evidence
* Human authority boundaries
* Important product commitments
* Important architecture commitments

Everything downstream should be traceable back to a North Star or an explicitly versioned change to it.

Agents must not silently rewrite the North Star.

A material goal change should create a new version and require the appropriate authority.

## 5. Waystar

Waystar is the understanding and design system.

Its job is to turn incomplete human intent into a sufficiently understood problem.

Waystar may:

* Grill the human
* Surface assumptions
* Identify missing requirements
* Model users and journeys
* Analyse business value
* Analyse architecture
* Find contradictions
* Expose risks
* Identify unknowns
* Request evidence
* Reconcile business and architecture views
* Record decisions
* Prepare execution ready understanding

Waystar should remain separated from implementation authority.

Its natural boundary is:

```text
Messy intent
  ↓
Structured understanding
  ↓
Evidence
  ↓
Decisions
  ↓
Execution ready model
```

The runtime starts after that boundary, although runtime findings may send the project back through Waystar.

## 6. Waystar Research Superpower

Research should become a first class Waystar capability.

The question belongs to Waystar because research exists to reduce uncertainty in business, product, architecture, or planning.

The execution of research does not always need to belong to Waystar itself.

Two modes are useful.

### 6.1 Minor Research

Minor Research is a bounded research action.

Typical characteristics:

* One clear question
* Small source set
* Short time budget
* Limited tool use
* Usually one researcher
* No need for a swarm
* Result returned directly to the current Waystar reasoning flow

Examples:

* Confirm whether a framework supports a required capability
* Check one API contract
* Inspect a repository implementation
* Find an authoritative specification
* Validate a pricing assumption
* Compare two small technical options

Minor Research should produce a compact evidence packet containing the question, findings, sources, uncertainty, and relevance to the current decision.

### 6.2 Major Research

Major Research is deep research compiled into executable work.

Waystar owns the research question and defines what evidence would be sufficient.

The runtime owns execution.

A possible flow is:

```text
Waystar identifies an important unknown
  ↓
Research brief
  ↓
Research WorkSet
  ↓
Runtime
  ↓
Parallel research workers
  ↓
Independent source gathering
  ↓
Contradiction analysis
  ↓
Synthesis
  ↓
Verification
  ↓
Research Evidence Packet
  ↓
Waystar updates the design model
```

Major Research can use multiple harnesses, models, search providers, repository inspection tools, plugins, files, or specialist skills.

It should be able to divide a large question into independent research lines, execute them in parallel, compare findings, identify disagreement, and synthesise an evidence bound conclusion.

This is an important example of the overall Constellation boundary:

> Waystar owns the question. Runtime owns the execution. Capabilities gather evidence. Verification challenges the result. Chronicle records what happened.

### 6.3 Research escalation

Minor Research should be able to escalate into Major Research when the system detects conditions such as:

* High uncertainty
* Conflicting evidence
* Large search space
* High decision impact
* Multiple independent questions
* Weak source quality
* Need for parallel investigation
* Need for independent synthesis

Escalation should be explicit and budget aware.

## 7. Capability Registry

Skills, plugins, tools, models, workflows, evaluators, and harness features should be represented as capabilities.

Examples include:

* Superpowers
* GitHub
* Web research
* MCP servers
* Repository analysers
* Architecture review skills
* Security review skills
* Test generation
* Code generation
* Model routers
* Domain specific plugins
* Human review
* External services

A capability definition should eventually describe:

* Identity
* Provider
* Version
* Capability type
* Inputs
* Outputs
* Permissions
* Network requirements
* Write authority
* Cost characteristics
* Trust level
* Harness compatibility
* Evidence produced
* Failure behaviour

Capabilities may augment, replace, or compose with other capabilities.

For example:

```text
Need: specification review

Possible providers:
  Waystar review workflow
  Superpowers specification skill
  External architecture review skill
  Human review
```

Policy and context determine which provider is used.

## 8. Adaptive Workflows

Constellation should not hardcode one giant workflow.

Workflows should be compositions of capabilities.

A substantial new product might use:

```text
Discovery
  ↓
Waystar Business
  ↓
Minor Research
  ↓
Waystar Architecture
  ↓
Major Research
  ↓
External Specification Review
  ↓
Alignment
  ↓
Execution Compilation
```

A small bug may use:

```text
Repository inspection
  ↓
WorkSet
  ↓
Implement
  ↓
Verify
```

An existing mature specification may bypass discovery entirely.

Workflow selection may therefore adapt to:

* Project size
* Existing evidence
* Risk
* Regulatory context
* Uncertainty
* Available capabilities
* Human preference
* Budget
* Time pressure

Adaptive does not mean uncontrolled.

Workflow changes should remain visible and explainable.

## 9. WorkSets

WorkSet is the proposed contract between understanding and execution.

Waystar produces execution ready understanding.

The WorkSet Compiler turns that understanding into bounded executable work.

A WorkSet may contain:

* Objective
* Source goals
* Source requirements
* Dependencies
* WorkItems
* Acceptance contracts
* Evidence requirements
* Required capabilities
* Permitted harnesses
* Concurrency rules
* Risk level
* Authority requirements
* Budget
* Retry policy
* Stop conditions
* Context requirements

A WorkItem is a bounded unit of executable responsibility inside a WorkSet.

WorkSets should not encode one delivery methodology.

A graph can naturally contain:

* Sequential work
* Parallel work
* Research spikes
* Hard gates
* Rolling refinement
* Independent experiments
* Integration points
* Human approval points

This allows a project to behave like waterfall where dependencies require strict sequencing and like agile where independent work can evolve concurrently.

The dependency graph determines the shape.

## 10. Context Projector

The Context Projector produces the minimum sufficient context for a worker or manager.

A worker should usually receive:

* Relevant North Star information
* Relevant requirements
* Its WorkItem
* Immediate dependencies
* Applicable decisions
* Relevant findings
* Required capability instructions
* Acceptance criteria
* Permitted write scope
* Relevant evidence

It should not receive unrelated project history simply because that history exists.

The Context Projector is one of the main mechanisms that allows Constellation to scale without repeatedly copying the entire project into every model call.

Semantic compression and Semantic Packetisation may eventually become useful implementation strategies here.

## 11. Constellation Runtime

The runtime executes WorkSets while preserving Constellation policy.

Its responsibilities include:

* Scheduling
* Concurrency
* Worker allocation
* Harness selection
* Model selection
* Capability resolution
* Retries
* Circuit breakers
* Deadlines
* Budgets
* Locks
* Recovery
* Cancellation
* Acceptance
* Verification
* State transitions
* Runtime evidence capture

Workers do not own these policies.

A worker can report success. The runtime decides whether the declared acceptance contract has been satisfied.

The current Constellation PoC explores this boundary through bounded workers, isolated worktrees, explicit state, circuit breakers, recovery, and independent acceptance.

## 12. Harness Adapters

Constellation should execute through adapters rather than becoming one more proprietary agent harness.

A conceptual adapter may support operations such as:

```text
discover capabilities
execute bounded work
stream observations
collect artifacts
cancel
resume
report usage
report failure
```

Possible adapters include:

* Codex
* OpenCode
* Antigravity
* Hermes
* OpenClaw
* Future local or hosted harnesses

The adapter owns translation between the Constellation execution contract and the harness API.

The runtime retains ownership of why the work exists, what context is allowed, what counts as success, and what happens next.

## 13. Verification and Acceptance

Verification should be independent from worker claims.

An acceptance contract may require:

* Tests
* Static analysis
* Behavioural checks
* Architecture rules
* Evidence review
* Human approval
* Security checks
* Performance thresholds
* Integration checks
* Comparison with the original goal

Different WorkItems may require different combinations.

Passing verification proves the declared contract at that point in time. It does not imply universal correctness.

## 14. Chronicle

Chronicle should evolve from a Waystar concern into a Project Constellation wide history and evidence service.

Waystar writes to Chronicle.

Research writes to Chronicle.

Runtime writes to Chronicle.

Workers contribute findings and artifacts.

Verification writes outcomes.

Humans record decisions and approvals.

Chronicle should preserve events such as:

* Assumption created
* Requirement discovered
* Decision made
* Research requested
* Evidence added
* WorkSet created
* Worker started
* Finding discovered
* Verification failed
* Architecture assumption invalidated
* Change proposed
* Human decision recorded
* WorkSet superseded
* Result accepted

The goal is not merely audit logging.

Chronicle should make it possible to reconstruct why the current system exists and how understanding changed.

## 15. Adaptation

Execution will discover things that planning could not know.

Constellation therefore needs explicit adaptation levels.

### 15.1 Local adaptation

Examples:

* Retry
* Alternate implementation
* Different model
* Different skill
* Different worker

The runtime may usually handle this within policy.

### 15.2 WorkSet adaptation

Examples:

* Split a WorkItem
* Change dependencies
* Add a verification task
* Replace a capability
* Reorder work

This changes execution while preserving the underlying design.

### 15.3 Design adaptation

A product or architecture assumption has changed.

The finding should return to Waystar for analysis and decision.

Affected WorkSets can then be regenerated or superseded.

### 15.4 Goal adaptation

The North Star itself must change.

This is a human authority boundary unless explicitly delegated.

This hierarchy prevents a swarm from efficiently building the wrong system because an implementation agent silently changed the objective.

## 16. Swarms

Swarm is an execution strategy, not the fundamental architecture.

A small WorkSet may require one worker.

A medium WorkSet may expose several independent WorkItems and therefore run several workers concurrently.

A large WorkSet may require managers over bounded subgraphs.

Conceptually:

```text
Project Runtime
  ↓
Scope Manager
  ↓
Bounded subgraph
  ↓
Workers
```

A useful principle from the existing research is:

> A manager is a control loop over a bounded part of the graph.

Managers should receive projected context for their scope rather than global project context.

This makes recursive coordination possible without requiring every worker to know everything.

## 17. Viewer

The Viewer is the human projection of Project Constellation.

It should not own canonical state or execution logic.

It reads the system and issues authorised commands back to the control plane.

The Viewer should provide several projections over the same underlying model.

### 17.1 Intent and Readiness View

Shows:

* North Star
* Goals
* Requirements
* Constraints
* Decisions
* Open questions
* Missing evidence
* Missing capabilities
* Unresolved architecture
* Readiness for planning
* Readiness for execution

This is closest to the original hand drawn Constellation diagram.

The important feature is that it shows not only what exists, but what is still missing.

### 17.2 Plan View

Shows:

* WorkSets
* WorkItems
* Dependencies
* Parallel branches
* Critical path
* Blocked work
* Acceptance gates
* Human approval points

### 17.3 Runtime View

Shows:

* Active workers
* Current WorkItems
* Harness
* Model
* Capability usage
* Elapsed time
* Budget
* Retries
* Failures
* Verification state
* Progress
* Adaptations in progress

The graph should update as the runtime adapts.

### 17.4 Evidence View

Shows traceability from intent to accepted outcome.

Conceptually:

```text
Goal
  ↓
Requirement
  ↓
Decision
  ↓
WorkSet
  ↓
WorkItem
  ↓
Run
  ↓
Artifact
  ↓
Verification
  ↓
Accepted outcome
```

A powerful long term interaction would be:

> Why does this exist?

For a line of code, artifact, architectural component, or task, the Viewer should be able to walk backwards through evidence and decisions to the originating goal.

### 17.5 Chronicle View

Shows how assumptions, findings, decisions, and plans evolved through time.

This is the historical explanation of the current graph.

### 17.6 Controls

The Viewer may expose controls such as:

* Pause project
* Pause WorkSet
* Cancel worker
* Stop all workers
* Resume
* Approve change
* Reject change
* Request replan
* Force verification
* Change budget
* Pin model
* Pin harness
* Disable capability
* Reassign work
* Request human review

These controls should issue commands through the same policy and authority model used everywhere else.

## 18. Canonical domain model

The exact schema is not yet fixed, but the current conceptual objects are:

### Intent and understanding

* NorthStar
* Goal
* Requirement
* Constraint
* Assumption
* Question
* Decision

### Research and evidence

* ResearchRequest
* ResearchResult
* Evidence
* Source
* Finding
* Uncertainty

### Capability and workflow

* Capability
* CapabilityProvider
* Workflow
* Policy
* AuthorityGrant
* Budget

### Planning and execution

* WorkSet
* WorkItem
* Dependency
* AcceptanceContract
* ContextProjection
* Run

### Change and verification

* ChangeProposal
* Artifact
* Verification
* Acceptance

### History and projection

* ChronicleEvent
* Projection

The first major design task should be deciding which of these objects are canonical and which are derived projections.

## 19. Repository boundaries

The current repositories should remain separate while the contracts are still evolving.

### Constellation

The public umbrella and concept space.

It should hold the project thesis, shared concepts, architectural boundaries, research notes, and eventually stable cross component contracts.

### Constellation Waystar

The understanding, research, business reasoning, architecture reasoning, alignment, Chronicle interaction, and execution compilation system.

Waystar Research should be developed here initially because research questions originate from understanding and design uncertainty.

### Constellation PoC

The current runtime experiment.

It explores execution, bounded workers, state, verification, recovery, circuit breakers, context projection, and harness integration.

It may eventually become Constellation Runtime if the architecture survives experimentation.

### Viewer

Viewer should probably remain a concept until the canonical graph and event contracts are stable enough to project reliably.

A separate implementation can emerge later.

The repos should integrate through explicit contracts rather than being merged into one monolith.

## 20. Example end to end journey

A substantial project could evolve like this:

```text
Human idea
  ↓
North Star created
  ↓
Waystar grills the human
  ↓
Requirements and assumptions emerge
  ↓
Minor Research resolves small unknowns
  ↓
Major Research compiles important unknowns into Research WorkSets
  ↓
Runtime executes research in parallel
  ↓
Evidence returns to Waystar
  ↓
Product and architecture models mature
  ↓
External skills review selected parts
  ↓
Alignment resolves conflicts
  ↓
Execution Compiler creates WorkSets
  ↓
Runtime resolves capabilities and harnesses
  ↓
Context Projector creates bounded worker context
  ↓
Workers execute in parallel where dependencies allow
  ↓
Verification checks candidate outcomes
  ↓
Findings trigger local, WorkSet, design, or goal adaptation
  ↓
Accepted outputs accumulate
  ↓
Chronicle records the full reasoning and execution history
  ↓
Viewer shows the live state and gives the human control
```

## 21. What Project Constellation is not

It is not intended to be:

* One universal agent
* One fixed workflow
* A replacement for Codex, OpenCode, OpenClaw, Hermes, or other harnesses
* A requirement that every task uses a swarm
* A requirement that every task uses Waystar
* A giant global prompt
* An autonomous system with unlimited authority
* A dashboard that merely watches external agents
* A claim that one delivery methodology fits every project

It is the connective system that preserves intent, state, evidence, authority, and adaptation across those components.

## 22. Open design questions

Important questions still to resolve include:

1. What is the minimum canonical graph?
2. Which domain objects must be persisted and which should be projections?
3. What is the minimum WorkSet schema?
4. How should capability discovery and compatibility work?
5. How are capabilities trusted and versioned?
6. How should external skills declare permissions and evidence?
7. How should workflow selection adapt without becoming opaque?
8. What is the precise boundary between Waystar and the WorkSet Compiler?
9. What is the precise boundary between WorkSet planning and runtime scheduling?
10. How should Minor Research escalate into Major Research?
11. How should research source quality and contradiction be represented?
12. How should the Context Projector be evaluated?
13. What is the common harness adapter contract?
14. How should cancellation, resume, and uncertain external effects be represented across different harnesses?
15. How should Chronicle events link design and execution histories?
16. How should Viewer subscribe to state without becoming another source of truth?
17. How should cost, quota, and model routing policies interact with acceptance risk?
18. When does a bounded subgraph need its own manager?
19. How should a change propagate to already running WorkSets?
20. How much of the cross component contract should become a formal protocol?

## 23. Near term experiments

The next useful experiments are:

1. Define the minimum NorthStar schema.
2. Define the minimum WorkSet and WorkItem contracts.
3. Define a capability manifest.
4. Add Waystar Minor Research as a bounded evidence producing capability.
5. Add Waystar Major Research that compiles a Research WorkSet and delegates execution to Runtime.
6. Define a minimal harness adapter contract around execute, observe, cancel, artifacts, usage, and failure.
7. Make the existing runtime consume a WorkSet shaped contract rather than fixture specific planning.
8. Add Chronicle links across research, design, planning, and execution.
9. Build a read only Viewer projection over a saved graph before adding control operations.
10. Prove one complete vertical slice from human goal to accepted implementation with full traceability.

## 24. Working vocabulary

The current vocabulary is intentionally provisional.

**Project Constellation**

The whole ecosystem.

**North Star**

The durable project intent and success definition.

**Waystar**

The understanding, research, product, architecture, and alignment system.

**Research Superpower**

Waystar capability for Minor Research and Major Research.

**Capability**

A discoverable unit of useful behaviour provided by a skill, plugin, tool, model, workflow, harness, or human.

**WorkSet**

A bounded executable plan connected to goals, dependencies, evidence, policy, and acceptance.

**Context Projector**

The mechanism that selects minimum sufficient context for a bounded responsibility.

**Constellation Runtime**

The execution control plane.

**Harness Adapter**

The boundary between Runtime and an external execution environment.

**Chronicle**

The durable history of evidence, decisions, findings, and state transitions.

**Viewer**

The human projection and control surface over the Constellation graph.

## 25. Core statement

Project Constellation should make it possible to start with an incomplete human idea and end with a verified result while preserving the chain between them.

The system should be able to explain:

* What are we trying to achieve?
* What do we know?
* What do we not know?
* What evidence supports the current design?
* What capabilities are available?
* What capabilities are missing?
* What work exists?
* What can run in parallel?
* What is blocked?
* What is running now?
* What changed?
* Why did it change?
* What evidence says a result is acceptable?
* Who authorised the important decisions?
* Why does this artifact exist?

If those questions can be answered from the same connected model, Constellation becomes more than an orchestrator.

It becomes an operating system for turning intent into evidence bound execution.
