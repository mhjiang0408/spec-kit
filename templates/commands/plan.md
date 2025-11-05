---
description: Execute the implementation planning workflow using the plan template to generate comprehensive design artifacts with multi-level task decomposition and consequence prediction.
scripts:
  sh: scripts/bash/setup-plan.sh --json
  ps: scripts/powershell/setup-plan.ps1 -Json
agent_scripts:
  sh: scripts/bash/update-agent-context.sh __AGENT__
  ps: scripts/powershell/update-agent-context.ps1 -AgentType __AGENT__
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Outline

1. **Setup**: Run `.specify/scripts/bash/setup-plan.sh --json` from repo root and parse JSON for FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. For single quotes in args like "I'm Groot", use escape syntax: e.g 'I'\''m Groot' (or double-quote if possible: "I'm Groot").

2. **Load context**: Read FEATURE_SPEC and `.specify/memory/constitution.md`. Load IMPL_PLAN template (already copied).

3. **Execute plan workflow**: Follow the structure in IMPL_PLAN template to:
   - Fill Technical Context (mark unknowns as "NEEDS CLARIFICATION")
   - Fill Constitution Check section from constitution
   - Evaluate gates (ERROR if violations unjustified)
   - Phase 0: Generate research.md (resolve all NEEDS CLARIFICATION)
   - Phase 1: Generate hierarchical task breakdown with consequence prediction
   - Phase 2: Generate data-model.md, contracts/, quickstart.md
   - Phase 3: Update agent context by running the agent script
   - Re-evaluate Constitution Check post-design

4. **Stop and report**: Command ends after Phase 3 planning. Report branch, IMPL_PLAN path, and generated artifacts.

## Phases

### Phase 0: Outline & Research

1. **Extract unknowns from Technical Context** above:
   - For each NEEDS CLARIFICATION → research task
   - For each dependency → best practices task
   - For each integration → patterns task

2. **Generate and dispatch research agents**:
   ```
   For each unknown in Technical Context:
     Task: "Research {unknown} for {feature context}"
   For each technology choice:
     Task: "Find best practices for {tech} in {domain}"
   ```

3. **Consolidate findings** in `research.md` using format:
   - Decision: [what was chosen]
   - Rationale: [why chosen]
   - Alternatives considered: [what else evaluated]
   - Performance implications: [expected impact]
   - Security considerations: [potential risks]

**Output**: research.md with all NEEDS CLARIFICATION resolved

### Phase 1: Hierarchical Task Breakdown & Consequence Analysis

**Prerequisites:** `research.md` complete

This phase decomposes ALL features into a three-tier hierarchy. Testing implementation is NOT considered a feature but a verification step.

#### 1.1 Feature Classification

Classify each feature from the spec into:

- **Large Feature**: Complex builds requiring manual input, deep research, and extensive back-and-forth
- **Medium Feature**: Multi-file work requiring review steps (typically by Kieran)
- **Small Feature**: Simple enough to complete in one session

#### 1.2 Hierarchical Decomposition

For each **Large Feature**:
1. Break down into **Medium Features** (2-5 components)
2. For each Medium Feature, break down into **Small Features** (3-8 atomic tasks)
3. Each Small Feature must be:
   - Completable in one session
   - Have clear input/output
   - Be independently testable
   - Have explicit dependencies listed

**Decomposition Template**:

```markdown
## Large Feature: [Name]

### Overview
- Description: [What this achieves]
- Business value: [Why this matters]
- Dependencies: [External requirements]

### Medium Features

#### MF-1: [Medium Feature Name]
- Scope: [What it covers]
- Review required: Yes/No
- Reviewer: [Name if known]

##### Small Features

###### SF-1.1: [Small Feature Name]
- **Description**: [Concrete task]
- **Files affected**: [List specific files]
- **Dependencies**: [Other SFs that must complete first]
- **Acceptance criteria**: 
  - [ ] Criterion 1
  - [ ] Criterion 2
- **Estimated complexity**: [Low/Medium/High]

###### [Predict-2]: Second-Order Consequences
- **Performance impact**: 
  - Database queries: [Expected query count, complexity]
  - Memory usage: [Expected increase/pattern]
  - Response time: [Expected latency change]
  - Network calls: [External API calls added]
- **System coupling**: 
  - New dependencies introduced: [List]
  - Modules affected: [Downstream impacts]
- **Data flow changes**: 
  - New data paths: [Describe]
  - Caching implications: [Cache invalidation needs]
- **Scalability concerns**: 
  - Bottlenecks: [Potential limits]
  - Concurrency issues: [Race conditions, locks]

###### [Predict-3]: Third-Order Consequences
- **Maintenance burden**: 
  - Code complexity increase: [Technical debt added]
  - Documentation needs: [What must be documented]
- **Future feature blockers**: 
  - Constraints introduced: [Limitations for future work]
  - Refactoring triggers: [When this will need revision]
- **Cross-team impacts**: 
  - API contract changes: [Breaking changes]
  - Integration points: [Other teams affected]
- **Operational impacts**: 
  - Monitoring needs: [New metrics required]
  - Alerting requirements: [New alerts]
  - Deployment complexity: [Migration steps]
- **User experience ripples**: 
  - Edge cases introduced: [New error states]
  - UX consistency: [Pattern deviations]

###### Risk Mitigation
- **For [Predict-2] issues**: [Specific solutions]
- **For [Predict-3] issues**: [Preventive measures]
- **Monitoring plan**: [How to detect issues]
- **Rollback strategy**: [How to revert if needed]
```

#### 1.3 Consequence Prediction Rules

For **every Small Feature**, you MUST analyze:

**[Predict-2] - Immediate Technical Consequences:**
- How does data retrieval/storage change?
- What's the performance profile? (O(n), O(n²), etc.)
- Are there new external dependencies?
- Does this introduce caching needs?
- Will this create database connection pressure?
- Are there N+1 query risks?
- Does this add significant memory allocation?

**[Predict-3] - Downstream & Long-term Consequences:**
- Does this create technical debt?
- Will this complicate future features?
- Does this introduce new failure modes?
- Are there security implications?
- Will this require additional monitoring?
- Does this affect system observability?
- Are there compliance/audit implications?
- Will this necessitate future refactoring?

**Critical Check**: If a Small Feature's [Predict-2] analysis reveals a performance issue (e.g., "slow database extraction"), you MUST:
1. Flag it as **HIGH RISK**
2. Propose alternative implementations
3. Add a verification step to measure actual impact
4. Document the acceptable performance threshold

#### 1.4 Dependency Graph

Generate a visual dependency graph showing:
- Small Features and their dependencies
- Critical path (features blocking the most downstream work)
- Parallelizable features (can be worked on simultaneously)

**Output**: `task-breakdown.md` with complete hierarchy and consequence analysis

### Phase 2: Design & Contracts

**Prerequisites:** `task-breakdown.md` complete

1. **Extract entities from feature spec** → `data-model.md`:
   - Entity name, fields, relationships
   - Validation rules from requirements
   - State transitions if applicable
   - **Performance annotations**: Expected record volumes, query patterns
   - **Indexing strategy**: Based on Predict-2 analysis

2. **Generate API contracts** from functional requirements:
   - For each user action → endpoint
   - Use standard REST/GraphQL patterns
   - Include rate limiting requirements (from Predict-2)
   - Document expected payload sizes
   - Output OpenAPI/GraphQL schema to `/contracts/`

3. **Create quickstart.md**:
   - Setup instructions
   - Key architectural decisions (linked to research.md)
   - **Performance baselines**: Expected metrics
   - **Known limitations**: From Predict-3 analysis

**Output**: data-model.md, /contracts/*, quickstart.md

### Phase 3: Agent Context Update

**Prerequisites:** All Phase 2 artifacts complete

1. **Agent context update**:
   - Run `.specify/scripts/bash/update-agent-context.sh codex`
   - These scripts detect which AI agent is in use
   - Update the appropriate agent-specific context file
   - Add only new technology from current plan
   - Preserve manual additions between markers
   - **Include performance constraints** from Predict-2 analysis
   - **Include architectural warnings** from Predict-3 analysis

**Output**: Agent-specific context file updated

## Validation Gates

Before completing each phase, validate:

### Phase 1 Gates
- [ ] Every Large Feature decomposed to Medium Features
- [ ] Every Medium Feature decomposed to Small Features
- [ ] Every Small Feature has [Predict-2] analysis
- [ ] Every Small Feature has [Predict-3] analysis
- [ ] All HIGH RISK items have mitigation plans
- [ ] Dependency graph shows no circular dependencies
- [ ] Critical path identified

### Phase 2 Gates
- [ ] Data model addresses all Predict-2 performance concerns
- [ ] API contracts include rate limiting for risky endpoints
- [ ] Quickstart documents all architectural trade-offs

### Phase 3 Gates
- [ ] Agent context includes all performance constraints
- [ ] Constitution check passes with design artifacts
- [ ] No unresolved NEEDS CLARIFICATION items

## Key Rules

- Use absolute paths
- ERROR on gate failures or unresolved clarifications
- EVERY feature MUST have consequence analysis
- Testing is NOT a feature; it's a verification step
- Small Features must be atomic and completable in one session
- [Predict-2] and [Predict-3] are MANDATORY for all Small Features
- Flag and resolve performance issues during planning, not implementation

## Output Summary

Final deliverables:
1. `research.md` - All technical unknowns resolved
2. `task-breakdown.md` - Complete hierarchy with consequence prediction
3. `data-model.md` - Entities with performance annotations
4. `/contracts/*` - API contracts with safeguards
5. `quickstart.md` - Setup guide with constraints
6. Updated agent context files