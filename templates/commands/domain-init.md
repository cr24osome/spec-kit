---
description: Initialize domain model with bounded contexts, ubiquitous language, and AI-optimized domain artifacts for better code generation.
handoffs: 
  - label: Create Constitution from Domain
    agent: speckit.constitution
    prompt: Create constitution with domain-driven principles from the domain model
  - label: Create Feature in Bounded Context
    agent: speckit.specify
    prompt: Create a feature specification within the [specify bounded context name] bounded context
  - label: Update Existing Specs
    agent: speckit.specify
    prompt: Review and update existing specifications to align with the new domain model
scripts:
  sh: .specify/scripts/bash/setup-domain.sh --json "{ARGS}"
  ps: .specify/scripts/powershell/setup-domain.ps1 -Json "{ARGS}"
---

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

## Purpose

This command creates a comprehensive domain model that serves as the **foundation for all AI-driven development**. It generates artifacts specifically designed to help AI agents understand:

- Business terminology (Ubiquitous Language)
- System boundaries (Bounded Contexts)
- Core concepts (Entities, Value Objects, Aggregates)
- Integration patterns (Events, APIs)
- Implementation guidance (Code structure, patterns, rules)

## Outline

You are creating a comprehensive domain model at `.specify/domain/domain.md`. This file starts as a TEMPLATE with placeholder tokens (e.g., `[PROJECT_NAME]`, `[CONTEXT_1_NAME]`). Your job is to:
1. Run the setup script to initialize the template
2. Analyze the domain description
3. Replace placeholders incrementally (like the constitution command)
4. Validate completeness
5. Report results

Follow this execution flow:

## Execution Steps

### Step 1: Run Setup Script

Execute `{SCRIPT}` to initialize domain structure:

```bash
{SCRIPT}
```

**The script creates**:
- `.specify/domain/` directory
- `.specify/domain/domain.md` (copied from template)
- `.specify/domain/entities.md` (entity catalog stub)
- `.specify/domain/events.md` (event catalog stub)
- `.specify/domain/context-map.md` (context map stub)

**Parse JSON output** to get the `domain_file` path (absolute path to domain.md).

### Step 2: Load Domain Template

Load `.specify/domain/domain.md` (created in Step 1) and identify placeholder tokens:

**Expected placeholders** (look for `[ALL_CAPS_TOKENS]`):
- `[PROJECT_NAME]` - Name of the project/system
- `[CONTEXT_1_NAME]`, `[CONTEXT_2_NAME]`, etc. - Bounded context names
- `[CONTEXT_1_PURPOSE]` - What each context does
- `[ENTITY_1_NAME]`, `[ENTITY_2_NAME]` - Entity names
- `[EVENT_1_NAME]`, `[EVENT_2_NAME]` - Domain event names
- Plus many more throughout the template

**IMPORTANT**: The template may have 3-5 context placeholders by default. Based on your domain analysis, you may need fewer or more contexts. Adjust the template accordingly.

### Step 3: Analyze Domain Description

Parse the user's input (from `$ARGUMENTS`) to extract domain concepts:

**Identify Bounded Contexts** (2-5 business capabilities):
- Look for distinct areas of responsibility
- E.g., "Order Management", "Inventory", "Payment", "Shipping"
- Each handles one cohesive capability
- Prioritize: P1 (core), P2 (important), P3 (supporting)

**Extract Ubiquitous Language** (10-20 key terms):
- Nouns → Entities/Value Objects (Customer, Order, Money)
- Verbs → Operations (PlaceOrder, ProcessPayment, ShipPackage)
- Identify synonyms to avoid ("Customer" not "User")

**Identify Entities** (5-10 things with identity):
- Have unique ID and lifecycle
- E.g., Customer, Order, Product, Payment, Shipment

**Identify Value Objects** (3-7 immutable concepts):
- No identity, defined by attributes
- E.g., Money, Address, OrderStatus, DateRange

**Identify Aggregates** (3-5 consistency boundaries):
- Transaction boundaries with root entity
- E.g., Order (root) + OrderLineItems

**Identify Domain Events** (5-10 significant occurrences):
- Past tense, business-relevant state changes
- E.g., OrderPlaced, PaymentCompleted, ProductShipped

### Step 4: Replace Placeholders Incrementally

**DO NOT try to generate the entire file at once.** Work section by section, replacing placeholders:

#### Phase 1: Basic Information
1. Replace `[PROJECT_NAME]` with the system name
2. Replace `[BUSINESS_DOMAIN]` with the industry/domain
3. Fill `[PRIMARY_GOAL]` and `[KEY_STAKEHOLDERS]`

#### Phase 2: Ubiquitous Language Section
1. Replace the terms table with 10-20 actual terms from your analysis
2. Add domain concepts and relationships
3. List terms to avoid
4. Keep the table format from template

#### Phase 3: Bounded Contexts Section
1. Count how many contexts you identified (2-5)
2. If template has more placeholders than needed, remove extras
3. If you need more, duplicate the context section structure
4. For EACH context, replace:
   - `[CONTEXT_N_NAME]` → Actual context name
   - `[CONTEXT_N_PURPOSE]` → What it does
   - `[CONTEXT_N_ENTITIES]` → List 3-5 core entities
   - `[CONTEXT_N_OPERATIONS]` → List 3-5 key operations
   - Boundaries and integration points

#### Phase 4: Domain Entities Section
1. For EACH entity (5-10 total), replace:
   - `[ENTITY_N_NAME]` → Entity name
   - `[ENTITY_N_ID]` → Identity attribute
   - `[ENTITY_N_ATTRIBUTES]` → Key attributes
   - `[ENTITY_N_INVARIANTS]` → Business rules
   - Lifecycle, relationships, behaviors

#### Phase 5: Value Objects Section
1. List value objects from your analysis
2. Replace `[VALUE_OBJECT_EXAMPLES]` with concrete ones
3. Add validation rules for each

#### Phase 6: Aggregates Section
1. For EACH aggregate (3-5 total), replace:
   - `[AGGREGATE_N_ROOT]` → Root entity name
   - `[AGGREGATE_N_PARTS]` → Contained entities/VOs
   - `[AGGREGATE_N_BOUNDARY]` → Transaction scope
   - Protection rules

#### Phase 7: Domain Events Section
1. For EACH event (5-10 total), replace:
   - `[EVENT_N_NAME]` → Event name (past tense)
   - `[EVENT_N_TRIGGER]` → When it happens
   - `[EVENT_N_DATA]` → Payload schema
   - `[EVENT_N_CONSUMERS]` → Which contexts listen
   - Integration pattern

#### Phase 8: Context Integration Section
1. Replace `[INTEGRATION_PATTERNS]` with actual patterns
2. Document event flows between contexts
3. Identify any shared kernels or anti-corruption layers

#### Phase 9: Implementation Guidance
1. Replace `[CODE_STRUCTURE]` with directory layout
2. Add naming conventions per context
3. List common patterns to use
4. Define error handling strategy

#### Phase 10: Remaining Sections
1. Fill Business Rules section
2. Add Data Flow Diagrams for key use cases
3. Complete Error Handling Strategy
4. Define Performance targets
5. Outline Testing Strategy  
6. Provide AI Agent DO/DON'T lists

**After each phase**: Write the updated domain.md back to disk before proceeding to next phase.

### Step 5: Update Supplementary Files

After domain.md is complete, update the stub files:

#### A. Update `entities.md`
- List all entities in a table
- Group by bounded context
- Include identity type and key attributes

#### B. Update `events.md`
- List all domain events
- Include trigger, data, consumers
- Show event flow diagrams

#### C. Update `context-map.md`
- Visual representation of context relationships
- Integration patterns between contexts
- Dependencies and data flows

### Step 6: Validation

Check against AI Implementation Readiness criteria:

✅ **Must Have**:
- [ ] 10+ ubiquitous language terms defined
- [ ] 2-5 bounded contexts with clear boundaries
- [ ] 5+ entities with behaviors (not anemic data structures)
- [ ] Invariants stated for each entity
- [ ] 5+ domain events defined
- [ ] Integration patterns between contexts specified
- [ ] No remaining `[PLACEHOLDER]` tokens

❌ **Must NOT Have**:
- [ ] Database schema details
- [ ] Framework-specific code  
- [ ] Infrastructure concerns
- [ ] Technical implementation in business sections

### Step 7: Report Completion

Provide final summary:

```
✅ Domain Model Created: .specify/domain/domain.md

📊 Domain Summary:
- Bounded Contexts: [count] ([list names])
- Core Entities: [count]
- Value Objects: [count]
- Aggregates: [count]
- Domain Events: [count]
- Integration Patterns: [list main patterns]

📁 Supplementary Files:
- .specify/domain/entities.md (entity catalog)
- .specify/domain/events.md (event catalog)
- .specify/domain/context-map.md (context relationships)

🤖 AI Implementation Readiness: READY
Domain model provides strategic context for AI development agent.

📝 Next Steps:
1. Review domain model: .specify/domain/domain.md
2. Create constitution: /speckit.constitution  
3. Create first feature: /speckit.specify [describe feature]
4. Implement high-priority contexts first

💡 Key Terms to Use in Code:
- [List 5 most important ubiquitous language terms]

🎯 Priority Implementation Order:
1. [P1 context] - [reason]
2. [P1 context] - [reason]
```

## Important Reminders

**Work Incrementally**:
- Run script first (Step 1)
- Load template (Step 2)
- Replace placeholders section by section (Steps 3-4)
- Write domain.md after each major phase
- Don't try to generate 800 lines at once

**Follow Template Structure**:
- Template already has good organization
- Just fill in business-specific details
- Keep AI Implementation Notes sections

**Adjust Context Count**:
- Template may have 3-5 context placeholders
- Add/remove as needed for your domain
- Duplicate structure if you need more contexts

## Guidelines for AI Domain Modeling

### Discovering Bounded Contexts

**Signs of a bounded context**:
- Different teams/roles work on it
- Uses different vocabulary for same concepts
- Has independent lifecycle
- Can change without affecting others
- Represents distinct business capability

**Examples**:
- E-commerce: Catalog, Orders, Fulfillment, Payments
- Healthcare: PatientRecords, Scheduling, Billing, Clinical
- Education: CourseManagement, StudentEnrollment, Grading, Content

### Creating Ubiquitous Language

**Process**:
1. Extract nouns from domain description
2. Group synonyms (Product = Item = Listing → choose one)
3. Define precisely in business terms
4. Document what NOT to call it
5. Specify where to use it

**Good entries**:
- ProductListing: A seller's item for sale | NOT: Product, Item, Thing | Use in: Catalog context
- Seller: User who creates ProductListings | NOT: Vendor, Merchant, User | Use in: Catalog, Orders

**Bad entries**:
- Item: A thing | NOT: Something | Use in: Everywhere
- Record: Database row | NOT: Entry | Use in: Code

### Defining Entities vs Value Objects

**Entity** (has identity):
- Can be tracked over time
- Has lifecycle (created → modified → deleted)
- Identity persists even if attributes change
- Examples: Customer, Order, Product

**Value Object** (no identity):
- Defined by its attributes
- Immutable
- Two instances with same values are equal
- Examples: Money, Address, DateRange

### Keeping Aggregates Small

**Good aggregates**:
- Order (root) + OrderItems
- Post (root) + Comments
- Invoice (root) + LineItems

**Bad aggregates** (too large):
- Order + OrderItems + Customer + Products + Payments + Shipment

**Rule**: If aggregate has >5-7 entities, split it

### Naming Domain Events

**Good names** (past tense, specific):
- OrderPlaced
- PaymentCompleted
- UserRegistered
- ProductPublished

**Bad names**:
- OrderEvent
- DoPayment
- UserChange
- Update

## Integration with SDD Workflow

After domain model is created:

1. **Constitution** (`/speckit.constitution`):
   Add domain-specific principles:
   - "Use ubiquitous language in all code"
   - "Enforce aggregate boundaries"
   - "Contexts communicate via events only"

2. **Feature Specs** (`/speckit.specify`):
   Reference domain model:
   - Specify bounded context
   - Use entity names
   - Document events triggered
   - Respect boundaries

3. **Implementation** (`/speckit.plan`, `/speckit.tasks`, `/speckit.implement`):
   Follow domain model:
   - Implement contexts as modules
   - Use suggested file structure
   - Enforce invariants
   - Publish events

## Example Output

For domain description:
```
"E-commerce platform for handmade crafts. Sellers create product listings with photos and descriptions. Buyers browse catalog, add items to cart, and checkout. Platform processes payments via Stripe and coordinates shipping."
```

Expected domain model includes:

**Bounded Contexts**:
- Catalog (P1): Product listings, search
- Orders (P1): Cart, checkout, order tracking  
- Payments (P1): Payment processing via Stripe
- Fulfillment (P2): Shipping coordination

**Ubiquitous Language**:
- ProductListing (not Product/Item)
- Seller (not Vendor/Merchant)
- Buyer (not Customer/User)
- Cart (not Basket/ShoppingCart)

**Key Entities**:
- ProductListing: id, title, description, price, seller
- Order: id, buyer, items, total, status
- Payment: id, amount, status, stripeTransactionId

**Domain Events**:
- ProductPublished
- OrderPlaced
- PaymentCompleted
- ShipmentCreated

**Integration**:
- Catalog → SearchIndex: ProductPublished event
- Orders → Payments: OrderPlaced event
- Payments → Orders: PaymentCompleted event
- Orders → Fulfillment: PaymentCompleted event

## Notes

- This is a **strategic design** activity, not tactical implementation
- Domain model is **living documentation** - update as understanding evolves
- Focus on **business concepts**, not technical implementation
- Optimize for **AI agent understanding** and code generation quality
- Review with **domain experts** to validate terminology and rules

