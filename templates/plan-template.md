# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]
**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command. See `.specify/templates/commands/plan.md` for the execution workflow.

## Summary

[Extract from feature spec: primary requirement + technical approach from research]

## Technical Context

<!--
  ACTION REQUIRED: Replace the content in this section with the technical details
  for the project. The structure here is presented in advisory capacity to guide
  the iteration process.
-->

**Language/Version**: [e.g., Python 3.11, Swift 5.9, Rust 1.75 or NEEDS CLARIFICATION]  
**Primary Dependencies**: [e.g., FastAPI, UIKit, LLVM or NEEDS CLARIFICATION]  
**Storage**: [if applicable, e.g., PostgreSQL, CoreData, files or N/A]  
**Testing**: [e.g., pytest, XCTest, cargo test or NEEDS CLARIFICATION]  
**Target Platform**: [e.g., Linux server, iOS 15+, WASM or NEEDS CLARIFICATION]  
**Project Type**: [single/web/mobile - determines source structure]  
**Performance Goals**: [domain-specific, e.g., 1000 req/s, 10k lines/sec, 60 fps or NEEDS CLARIFICATION]  
**Constraints**: [domain-specific, e.g., <200ms p95, <100MB memory, offline-capable or NEEDS CLARIFICATION]  
**Scale/Scope**: [domain-specific, e.g., 10k users, 1M LOC, 50 screens or NEEDS CLARIFICATION]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- Code Quality Discipline: Outline how the plan preserves modularity, documentation,
  and lint compliance for every deliverable.
- Comprehensive Testing Guarantees: Map unit and integration tests to each user
  story and describe CI execution.
- Consistent User Experience: Reference the design system, accessibility checklist,
  and UX validation steps that will be followed.
- Performance Reliability: Declare performance budgets and the telemetry that will
  confirm they are met.
- Operational Standards: Confirm uv-managed environment setup details and how
  protected data (e.g., `data/raw_data/`) will remain untouched.

## Project Structure

### Documentation (this feature)

```
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── task-breakdown.md    # Phase 1 output (/speckit.plan command)
├── data-model.md        # Phase 2 output (/speckit.plan command)
├── quickstart.md        # Phase 2 output (/speckit.plan command)
├── contracts/           # Phase 2 output (/speckit.plan command)
└── tasks.md             # Phase 3 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)
<!--
  ACTION REQUIRED: Replace the placeholder tree below with the concrete layout
  for this feature. Delete unused options and expand the chosen structure with
  real paths (e.g., apps/admin, packages/something). The delivered plan must
  not include Option labels.
-->

```
# [REMOVE IF UNUSED] Option 1: Single project (DEFAULT)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVE IF UNUSED] Option 2: Web application (when "frontend" + "backend" detected)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [REMOVE IF UNUSED] Option 3: Mobile + API (when "iOS/Android" detected)
api/
└── [same as backend above]

ios/ or android/
└── [platform-specific structure: feature modules, UI flows, platform tests]
```

**Structure Decision**: [Document the selected structure and reference the real
directories captured above]

## Hierarchical Task Breakdown

<!--
  ACTION REQUIRED: This section will be populated by Phase 1 of /speckit.plan
  and saved to task-breakdown.md. The structure below is a template showing
  the required format for feature decomposition.
-->

### Feature Classification Summary

- **Large Features**: [Count] - [List names]
- **Medium Features**: [Count] - [List names]
- **Small Features**: [Count] - [List names]

### Large Feature: [LF-1 Name]

#### Overview
- **Description**: [What this achieves]
- **Business Value**: [Why this matters]
- **Dependencies**: [External requirements]
- **Estimated Duration**: [Timeline]

#### Medium Features

##### MF-1.1: [Medium Feature Name]
- **Scope**: [What it covers]
- **Review Required**: Yes/No
- **Reviewer**: [Name if known]
- **Estimated Duration**: [Timeline]

###### Small Features

**SF-1.1.1: [Small Feature Name]**
- **Description**: [Concrete task]
- **Files Affected**: [List specific files]
- **Dependencies**: [Other SFs that must complete first]
- **Acceptance Criteria**: 
  - [ ] Criterion 1
  - [ ] Criterion 2
  - [ ] Criterion 3
- **Estimated Complexity**: Low/Medium/High

**[Predict-2]: Second-Order Consequences**
- **Performance Impact**: 
  - Database queries: [Expected query count, complexity, O(n) notation]
  - Memory usage: [Expected increase/pattern]
  - Response time: [Expected latency change with metrics]
  - Network calls: [External API calls added, timeout considerations]
- **System Coupling**: 
  - New dependencies introduced: [List with version constraints]
  - Modules affected: [Downstream impacts with specifics]
- **Data Flow Changes**: 
  - New data paths: [Describe with data volume estimates]
  - Caching implications: [Cache invalidation strategy needs]
- **Scalability Concerns**: 
  - Bottlenecks: [Potential limits with thresholds]
  - Concurrency issues: [Race conditions, locks, thread safety]

**[Predict-3]: Third-Order Consequences**
- **Maintenance Burden**: 
  - Code complexity increase: [Cyclomatic complexity, technical debt added]
  - Documentation needs: [What must be documented and where]
- **Future Feature Blockers**: 
  - Constraints introduced: [Limitations for future work with examples]
  - Refactoring triggers: [When this will need revision and why]
- **Cross-Team Impacts**: 
  - API contract changes: [Breaking changes with migration path]
  - Integration points: [Other teams/systems affected]
- **Operational Impacts**: 
  - Monitoring needs: [New metrics required with alert thresholds]
  - Alerting requirements: [New alerts with SLOs]
  - Deployment complexity: [Migration steps, rollback procedures]
- **User Experience Ripples**: 
  - Edge cases introduced: [New error states and handling]
  - UX consistency: [Pattern deviations and justification]

**Risk Mitigation**
- **For [Predict-2] Issues**: 
  - [Specific solution 1 with implementation details]
  - [Specific solution 2 with implementation details]
- **For [Predict-3] Issues**: 
  - [Preventive measure 1 with monitoring]
  - [Preventive measure 2 with documentation]
- **Monitoring Plan**: [How to detect issues - specific metrics and dashboards]
- **Rollback Strategy**: [How to revert if needed - specific steps]
- **Performance Threshold**: [If applicable - acceptable limits with justification]

---

**CRITICAL**: Repeat the above structure for ALL Medium and Small Features. Every Small Feature MUST have [Predict-2] and [Predict-3] sections filled out.

### Dependency Graph

```
[Visual representation or description of dependencies]

Critical Path: SF-X.X.X → SF-Y.Y.Y → SF-Z.Z.Z
Parallelizable Groups:
  - Group 1: [SF-A.A.A, SF-B.B.B]
  - Group 2: [SF-C.C.C, SF-D.D.D]

Blocking Features (blocking most downstream work):
  - SF-X.X.X blocks [count] features
  - SF-Y.Y.Y blocks [count] features
```

## Validation Gates

### Phase 0 Gates (Before Research)
- [ ] Constitution Check passes
- [ ] All NEEDS CLARIFICATION items identified
- [ ] Technical Context complete

### Phase 1 Gates (After Task Breakdown)
- [ ] Every Large Feature decomposed to Medium Features
- [ ] Every Medium Feature decomposed to Small Features
- [ ] Every Small Feature has [Predict-2] analysis
- [ ] Every Small Feature has [Predict-3] analysis
- [ ] All HIGH RISK items have mitigation plans
- [ ] Dependency graph shows no circular dependencies
- [ ] Critical path identified
- [ ] No unresolved NEEDS CLARIFICATION items from Phase 0

### Phase 2 Gates (After Design & Contracts)
- [ ] Data model addresses all Predict-2 performance concerns
- [ ] API contracts include rate limiting for risky endpoints
- [ ] Quickstart documents all architectural trade-offs
- [ ] All entity relationships validated
- [ ] Performance baselines documented

### Phase 3 Gates (After Agent Context Update)
- [ ] Agent context includes all performance constraints
- [ ] Agent context includes architectural warnings
- [ ] Constitution Check re-evaluated and passes
- [ ] All deliverables validated

## Complexity Tracking

*Fill ONLY if Constitution Check has violations that must be justified*

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |

## Constitution Re-evaluation

*GATE: Complete after Phase 2 artifacts are generated*

- [ ] Code Quality Discipline: [Status and notes]
- [ ] Comprehensive Testing Guarantees: [Status and notes]
- [ ] Consistent User Experience: [Status and notes]
- [ ] Performance Reliability: [Status and notes]
- [ ] Operational Standards: [Status and notes]

**Overall Status**: [PASS/FAIL with explanation]

---

## Key Rules for Plan Execution

- **MANDATORY**: All features MUST be decomposed into Small Features
- **MANDATORY**: All Small Features MUST have [Predict-2] and [Predict-3] analysis
- **MANDATORY**: Testing implementation is NOT a feature
- **MANDATORY**: Small Features must be completable in one session
- **MANDATORY**: Flag HIGH RISK items with mitigation before implementation
- **MANDATORY**: Use absolute paths in all file references
- **ERROR CONDITION**: Gate failures or unresolved NEEDS CLARIFICATION items