# Domain Model: [DOMAIN NAME]

**Created**: [DATE]  
**Status**: Draft  
**Input**: Domain description: "$ARGUMENTS"

## Executive Summary

[Brief overview of the domain, its purpose, and business context]

## Ubiquitous Language *(mandatory)*

<!--
  CRITICAL FOR AI: This section establishes the single source of truth for terminology.
  AI agents must use these exact terms in all code, specs, and documentation.
-->

### Core Terms

| Term | Definition | Synonyms to Avoid | Usage Context |
|------|------------|-------------------|---------------|
| [Term 1] | [Clear, unambiguous definition] | [Terms NOT to use] | [When/where to use this term] |
| [Term 2] | [Clear, unambiguous definition] | [Terms NOT to use] | [When/where to use this term] |
| [Term 3] | [Clear, unambiguous definition] | [Terms NOT to use] | [When/where to use this term] |

**AI Agent Guidance**: Use these exact terms in:
- Class and function names
- Variable names
- API endpoints
- Database table/column names
- User interface labels
- Documentation

### Domain Concepts

- **[Concept 1]**: [Explanation of key domain concept and its significance]
- **[Concept 2]**: [Explanation of key domain concept and its significance]
- **[Concept 3]**: [Explanation of key domain concept and its significance]

## Bounded Contexts *(mandatory)*

<!--
  CRITICAL FOR AI: Each bounded context represents a distinct subdomain.
  AI agents should implement each context as a separate module/service.
-->

### Context 1: [Context Name] (Priority: P1)

**Purpose**: [Why this context exists, what business capability it represents]

**Responsibilities**:
- [Responsibility 1]
- [Responsibility 2]
- [Responsibility 3]

**Core Entities**:
- [Entity 1] - [Brief description]
- [Entity 2] - [Brief description]

**Key Operations**:
- [Operation 1] - [What it does]
- [Operation 2] - [What it does]

**Boundaries**:
- **Owns**: [What data/concepts this context owns]
- **Does NOT handle**: [What is explicitly out of scope]

---

### Context 2: [Context Name] (Priority: P2)

**Purpose**: [Why this context exists]

**Responsibilities**:
- [Responsibility 1]
- [Responsibility 2]

**Core Entities**:
- [Entity 1] - [Brief description]
- [Entity 2] - [Brief description]

**Key Operations**:
- [Operation 1] - [What it does]
- [Operation 2] - [What it does]

**Boundaries**:
- **Owns**: [What data/concepts this context owns]
- **Does NOT handle**: [What is explicitly out of scope]

---

## Domain Entities *(mandatory)*

<!--
  CRITICAL FOR AI: These define the core data structures.
  Each entity should become a class/type with behavior, not just a data container.
-->

### [Bounded Context 1]

#### Entity: [Entity Name]

**Identity**: [What uniquely identifies this entity - e.g., UUID, email, composite key]

**Lifecycle**: [Created when] → [Key state transitions] → [Archived/Deleted when]

**Core Attributes**:
- `[attribute1]`: [type] - [description, constraints, business rules]
- `[attribute2]`: [type] - [description, constraints, business rules]
- `[attribute3]`: [type] - [description, constraints, business rules]

**Invariants** (rules that MUST always be true):
1. [Invariant 1 - e.g., "email must be unique"]
2. [Invariant 2 - e.g., "status can only transition from New → Active → Archived"]
3. [Invariant 3]

**Relationships**:
- [Relationship to other entity - e.g., "belongs to one Account"]
- [Relationship to other entity - e.g., "has many Comments"]

**Behavior** (Key methods this entity should have):
- `[method1]()` - [What it does, when to call it]
- `[method2]()` - [What it does, when to call it]

---

## Value Objects *(include if relevant)*

<!--
  CRITICAL FOR AI: Value objects are immutable and have no identity.
  Implement as frozen dataclasses or similar immutable structures.
-->

### [Value Object Name]

**Purpose**: [Why this is a value object, not an entity]

**Attributes**:
- `[attribute1]`: [type] - [description, constraints]
- `[attribute2]`: [type] - [description, constraints]

**Validation Rules**:
- [Rule 1 - e.g., "amount must be non-negative"]
- [Rule 2 - e.g., "currency must be valid ISO code"]

**Equality**: Two instances are equal when [equality criteria]

---

## Aggregates *(mandatory)*

<!--
  CRITICAL FOR AI: Aggregates define transaction boundaries.
  All changes to entities within an aggregate MUST go through the root.
-->

### Aggregate: [Aggregate Name]

**Root Entity**: [Entity that serves as entry point]

**Contains**:
- [Root entity]
- [Related entity or value object 1]
- [Related entity or value object 2]

**Consistency Boundary**: [What invariants does this aggregate protect?]

**Aggregate Rules**:
1. External objects can only reference the aggregate root
2. [Rule 2 - e.g., "All child entities must be accessed through root"]
3. [Rule 3 - e.g., "Aggregate commits as atomic unit"]

---

## Domain Events *(mandatory)*

<!--
  CRITICAL FOR AI: Domain events enable loose coupling between contexts.
  Publish these after successful aggregate commits.
-->

### [Bounded Context 1] Events

| Event Name | When Triggered | Data Included | Consumers | Integration Pattern |
|------------|----------------|---------------|-----------|---------------------|
| [Event 1] | [Trigger condition] | [Key data fields] | [Which contexts/systems] | [Pub/Sub, Message Queue, etc.] |
| [Event 2] | [Trigger condition] | [Key data fields] | [Which contexts/systems] | [Pub/Sub, Message Queue, etc.] |
| [Event 3] | [Trigger condition] | [Key data fields] | [Which contexts/systems] | [Pub/Sub, Message Queue, etc.] |

### [Bounded Context 2] Events

[Repeat for each context]

---

## Domain Services *(include if relevant)*

<!--
  CRITICAL FOR AI: Domain services handle operations that don't belong to any entity.
  These are NOT infrastructure services (like EmailService).
-->

### [Domain Service Name]

**Purpose**: [Why this service exists - what domain logic it encapsulates]

**Operations**:
- `[operation1]([params])` → [return type]
  - [What it does, when to use it, business rules involved]
- `[operation2]([params])` → [return type]
  - [What it does, when to use it, business rules involved]

**Collaborates With**:
- [Entity/Aggregate 1] - [How they interact]
- [Entity/Aggregate 2] - [How they interact]

---

## Context Mapping *(mandatory for multiple contexts)*

<!--
  CRITICAL FOR AI: This defines how contexts integrate.
  Never allow direct database access across context boundaries.
-->

### Context Relationships

```text
┌─────────────────┐         ┌─────────────────┐
│  [Context 1]    │────────>│  [Context 2]    │
│  (Upstream)     │  Events │  (Downstream)   │
└─────────────────┘         └─────────────────┘
        │
        │ Shared Kernel
        ▼
┌─────────────────┐
│  [Context 3]    │
│  (Partner)      │
└─────────────────┘
```

### Integration Patterns

| From Context | To Context | Pattern | Data Flow | Consistency Model |
|--------------|------------|---------|-----------|-------------------|
| [Context A] | [Context B] | Event Pub/Sub | [Event names] | Eventual |
| [Context B] | [Context C] | API Call | [Endpoints] | Strong |
| [Context C] | [Context A] | Shared Database | [Tables] | Strong - ⚠️ Anti-pattern |

### Anti-Corruption Layers

**Required for**: [List contexts that need translation layers]

**Purpose**: Protect domain model from external/legacy system models

---

## Business Rules *(mandatory)*

<!--
  CRITICAL FOR AI: These are cross-cutting rules that must be enforced consistently.
-->

### Category: [Rule Category - e.g., "Authorization"]

1. **[Rule Name]**: [Clear statement of rule]
   - **Applies to**: [Which entities/contexts]
   - **Enforcement**: [Where/how validated - e.g., aggregate, service, middleware]
   - **Exceptions**: [Any cases where rule doesn't apply]
   - **Error Handling**: [What to do when violated]

2. **[Rule Name]**: [Clear statement of rule]
   - **Applies to**: [Which entities/contexts]
   - **Enforcement**: [Where/how validated]
   - **Exceptions**: [Any cases where rule doesn't apply]
   - **Error Handling**: [What to do when violated]

---

## Data Flow Diagrams *(for AI understanding)*

### Use Case: [Primary Use Case Name]

```text
[User/System] → [Context 1] → [Event: X] → [Context 2] → [Result]
                     ↓
              [Database: Y]
```

**Steps**:
1. [Step 1 with entities involved]
2. [Step 2 with entities involved]
3. [Step 3 with entities involved]

---

## Review Checklist

Use this checklist to validate domain model completeness:

### Completeness
- [ ] All contexts have clear, non-overlapping responsibilities
- [ ] Ubiquitous language is defined and used consistently
- [ ] Entity identities and lifecycles are clear
- [ ] Aggregate boundaries protect important invariants
- [ ] Domain events capture all significant occurrences
- [ ] Context integration patterns are documented
- [ ] Business rules are explicitly stated

### Strategic Quality Gates
- [ ] No technical implementation details leak into domain model
- [ ] Model serves current business needs (not over-engineered)
- [ ] All synonyms are identified and avoided
- [ ] Context boundaries prevent coupling
- [ ] Aggregates are appropriately sized
- [ ] Domain services are truly domain logic (not infrastructure)

---

## Appendix: Domain Glossary

[Alphabetical reference of all domain terms for quick lookup]

| Term | Category | Definition | Context |
|------|----------|------------|---------|
| [Term] | [Entity/ValueObject/Event] | [Brief definition] | [Bounded Context] |

---

**Version**: 1.0  
**Last Updated**: [DATE]  
**Maintained By**: [Team/Person]  
**Purpose**: Strategic DDD model providing domain context for AI development agents

