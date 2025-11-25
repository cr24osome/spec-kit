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

[Gates determined based on constitution file]

## Domain Model Alignment *(include if domain model exists)*

<!--
  If `.specify/domain/domain.md` exists, complete this section.
  Otherwise, mark as "N/A - No domain model defined" and proceed to Project Structure.
-->

**Bounded Context**: [Which bounded context from `.specify/domain/domain.md` does this feature belong to?]

**Aggregate Boundaries**: [Which aggregate boundaries must be respected in this implementation?]
- [Aggregate 1]: [Rules for modification/access - e.g., "Can only be modified through Order aggregate root"]
- [Aggregate 2]: [Transaction boundaries - e.g., "Must maintain consistency within single transaction"]

**Domain Events Flow**: [Domain events this feature produces or consumes]

**Produces**:
- `[EventName]`: [When/why emitted] → [Which contexts consume this]
- `[EventName]`: [When/why emitted] → [Which contexts consume this]

**Consumes**:
- `[EventName]`: [From which context] → [How this feature reacts]
- `[EventName]`: [From which context] → [How this feature reacts]

**Ubiquitous Language Enforcement**: [Key domain terms and how code should reflect them]
- [Term 1] → [Code representation, e.g., "Use `PlaceOrder` method, not `CreateOrder` or `MakeOrder`"]
- [Term 2] → [Code representation, e.g., "Represent as `OrderStatus` enum with exact values from domain model"]

**Cross-Context Integration**: [How this feature integrates with other bounded contexts]
- [Context 1]: [Integration mechanism - e.g., "Via OrderPlaced event (async)", "Via REST API (sync)"]
- [Context 2]: [Integration mechanism and data flow]

**Domain Rules to Enforce**: [Critical business rules from domain model that code must implement]
1. [Rule 1, e.g., "Orders cannot be modified after shipment"]
2. [Rule 2, e.g., "Payment must be validated before order confirmation"]

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md              # This file (/speckit.plan command output)
├── research.md          # Phase 0 output (/speckit.plan command)
├── data-model.md        # Phase 1 output (/speckit.plan command)
├── quickstart.md        # Phase 1 output (/speckit.plan command)
├── contracts/           # Phase 1 output (/speckit.plan command)
└── tasks.md             # Phase 2 output (/speckit.tasks command - NOT created by /speckit.plan)
```

### Source Code (repository root)
<!--
  ACTION REQUIRED: Replace the placeholder tree below with the concrete layout
  for this feature. Delete unused options and expand the chosen structure with
  real paths (e.g., apps/admin, packages/something). The delivered plan must
  not include Option labels.
-->

```text
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

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations that must be justified**

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| [e.g., 4th project] | [current need] | [why 3 projects insufficient] |
| [e.g., Repository pattern] | [specific problem] | [why direct DB access insufficient] |
