# Domain-First Development with Spec Kit

## Overview

The `/speckit.domain` command creates a comprehensive domain model designed specifically to help AI coding agents understand your business domain and generate better code.

## Why Domain-First?

Traditional SDD starts with features. Domain-first SDD starts with understanding:

```
❌ Old Way: Feature → Spec → Plan → Code
✅ New Way: Domain → Feature → Spec → Plan → Code
```

**Benefits for AI Agents**:
- **Consistent terminology** across all generated code
- **Clear boundaries** between different system areas
- **Business rules** codified and enforced
- **Integration patterns** predefined
- **Code structure** guidance built-in

## Quick Start

### 1. Initialize Domain Model

```bash
uv run specify init my-project
cd my-project

# In your AI agent (Cursor, Claude, etc.):
/speckit.domain "E-commerce platform for handmade crafts. Sellers create product listings with photos. Buyers browse catalog and purchase items. Platform processes payments via Stripe and coordinates shipping."
```

### 2. Review Generated Artifacts

The command creates:
- `.specify/domain/domain.md` - Complete domain model (AI-optimized)
- `.specify/domain/entities.md` - Entity quick reference
- `.specify/domain/events.md` - Event catalog with schemas
- `.specify/domain/context-map.md` - Context relationships
- `.specify/domain/contracts/` - API contract templates

### 3. Use in Development

```bash
# Create domain-aware constitution
/speckit.constitution "Enforce domain-driven principles: use ubiquitous language, respect aggregate boundaries, contexts communicate via events only"

# Create features referencing domain
/speckit.specify "Allow sellers to create and publish product listings in the Catalog context"

# Implementation follows domain model
/speckit.plan
/speckit.tasks
/speckit.implement
```

## What Gets Generated

### Domain Model (domain.md)

A comprehensive 500+ line document with:

####  1. Ubiquitous Language
- **Core Terms Table**: Business vocabulary with definitions
- **Synonyms to Avoid**: Prevents terminology confusion
- **Usage Context**: Where each term applies
- **AI Guidance**: Exact terms to use in code

#### 2. Bounded Contexts (2-5 contexts)
Each context includes:
- Purpose and responsibilities
- Core entities (3-5 per context)
- Key operations
- Clear boundaries (owns vs. doesn't handle)
- **AI Implementation Notes**: Module names, API prefixes, file structure

#### 3. Domain Entities
For each entity:
- Identity (unique identifier)
- Lifecycle (creation → states → deletion)
- Core attributes with business rules
- **Invariants** (rules that MUST be true)
- Relationships to other entities
- **Behavior methods** (not anemic models!)
- **AI Implementation Notes**: File locations, validation patterns

#### 4. Value Objects
- Immutable concepts (Money, Address, DateRange)
- Validation rules
- Equality criteria
- **AI Implementation Notes**: Use frozen dataclasses

#### 5. Aggregates
- Transaction boundaries
- Root entity
- Contained entities
- Protected invariants
- **AI Implementation Notes**: Private members, public methods, atomicity

#### 6. Domain Events
- Event names (past tense)
- When triggered
- Data included
- Consumers
- Integration patterns
- **AI Implementation Notes**: Event schemas, publishing strategy

#### 7. Context Mapping
- Visual diagrams
- Integration patterns (Pub/Sub, API, etc.)
- Anti-corruption layers
- **AI Implementation Notes**: Message brokers, API contracts

#### 8. Business Rules
- Cross-cutting rules
- Where enforced
- Error handling
- **AI Implementation Notes**: Code location, validation approach

#### 9. Data Flow Diagrams
- Primary use cases
- Step-by-step flows
- API endpoints
- Event chains

#### 10. Error Handling Strategy
- Error categories
- HTTP status codes
- User messages
- Logging levels

#### 11. Performance Considerations
- Expected scale
- Response time targets
- Caching strategy
- Optimization priorities

#### 12. Testing Strategy
- Test pyramid (60% unit, 30% integration, 10% E2E)
- Test generation guidelines
- Test data strategies

#### 13. AI Agent Guidelines
- DO/DON'T lists
- Code organization structure
- Implementation patterns

### Supplementary Files

#### entities.md - Entity Catalog
Quick reference table of all entities:
- By bounded context
- Alphabetical index
- Identity, attributes, aggregates

#### events.md - Event Catalog
Complete event reference:
- Event index table
- JSON schemas for each event
- Triggers and consumers
- Integration patterns

#### context-map.md - Context Relationships
Visual representation:
- ASCII diagrams
- Integration matrix
- Dependency tracking

#### contracts/ - API Templates
Directory for API contract definitions per context

## Example: E-Commerce Platform

### Input

```bash
/speckit.domain "E-commerce platform. Sellers list products. Buyers browse and purchase. Platform handles payments and shipping."
```

### Output Structure

**Bounded Contexts**:
- **Catalog** (P1): Product listings, search, categories
- **Orders** (P1): Cart, checkout, order tracking
- **Payments** (P1): Payment processing
- **Fulfillment** (P2): Shipping coordination

**Ubiquitous Language**:
| Term | Definition | NOT Called |
|------|------------|------------|
| ProductListing | Seller's item for sale | Product, Item, Thing |
| Seller | User who creates listings | Vendor, Merchant |
| Buyer | User who purchases | Customer, User |

**Key Entities**:
- ProductListing (Catalog): id, title, price, seller, status
- Order (Orders): id, buyer, items[], total, status
- Payment (Payments): id, amount, status, transaction

**Domain Events**:
- ProductPublished → triggers SearchIndex update
- OrderPlaced → triggers Payment processing
- PaymentCompleted → triggers Fulfillment
- ShipmentCreated → updates Order status

**Integration**:
```
Catalog --[ProductPublished event]--> SearchIndex
Orders --[OrderPlaced event]--> Payments
Payments --[PaymentCompleted event]--> Fulfillment
```

**AI Code Structure**:
```
src/
├── catalog/
│   ├── models/
│   │   ├── product_listing.py
│   │   └── seller.py
│   ├── services/
│   │   └── catalog_service.py
│   ├── events/
│   │   └── product_published.py
│   └── repositories/
│       └── product_repository.py
├── orders/
│   └── ... (same structure)
└── payments/
    └── ... (same structure)
```

## How AI Agents Use This

### 1. Terminology Enforcement

**Without Domain Model**:
```python
class Item:  # ❌ Wrong term
    def make_live(self):  # ❌ Unclear
        self.state = "active"
```

**With Domain Model**:
```python
class ProductListing:  # ✅ From ubiquitous language
    def publish(self):  # ✅ Domain operation
        self.status = ListingStatus.PUBLISHED
        self.events.append(ProductPublished(...))
```

### 2. Boundary Respect

**Without Domain Model**:
```python
# ❌ Catalog directly accessing Orders database
listings = db.query("SELECT * FROM orders.product_listings")
```

**With Domain Model**:
```python
# ✅ Catalog owns ProductListings, Orders subscribes to events
def on_product_published(event: ProductPublished):
    # Orders context responds to Catalog events
    pass
```

### 3. Aggregate Enforcement

**Without Domain Model**:
```python
# ❌ Bypassing aggregate root
order.items.append(item)  # Direct access
order.total = calculate_total()  # Manual update
```

**With Domain Model**:
```python
# ✅ All access through aggregate root
order.add_item(product_id, quantity)  # Root method
# Total automatically updated, invariants maintained
```

### 4. Event-Driven Integration

**Without Domain Model**:
```python
# ❌ Direct coupling
def place_order(order):
    payment_service.charge(order.total)  # Tight coupling
    shipping_service.create_shipment(order)
```

**With Domain Model**:
```python
# ✅ Event-driven, loose coupling
def place_order(order):
    order.place()  # Changes state, emits OrderPlaced event
    event_bus.publish(order.get_events())  # Separate services respond
```

## When to Use Domain-First

### ✅ Use When:
- Complex business domain (>10 entities)
- Multiple system areas/capabilities
- Need consistent terminology
- Multiple developers/teams
- Long-term project (>3 months)
- Integration with other systems

### ❌ Skip When:
- Simple CRUD app (<5 entities)
- Single feature addition
- Prototype/MVP
- Time-critical delivery
- Domain already well-understood

## Integration with Existing Workflow

### Standard SDD
```
1. specify init
2. /speckit.constitution
3. /speckit.specify
4. /speckit.plan
5. /speckit.tasks
6. /speckit.implement
```

### Domain-First SDD
```
1. specify init
2. /speckit.domain ← NEW (strategic design)
3. /speckit.constitution (enhanced with domain principles)
4. /speckit.specify (domain-aware)
5. /speckit.plan (maps contexts to code)
6. /speckit.tasks
7. /speckit.implement (follows domain model)
```

## Best Practices

### 1. Start Simple
- Begin with 2-3 bounded contexts
- Add more as complexity grows
- Don't over-engineer

### 2. Involve Domain Experts
- Review terminology with business stakeholders
- Validate business rules
- Confirm context boundaries

### 3. Keep It Updated
- Domain model is living documentation
- Update as understanding evolves
- Review quarterly

### 4. Use Consistently
- Reference in all specs
- Enforce in code reviews
- Include in onboarding

### 5. Focus on Business
- Avoid technical jargon
- Use business language
- Model current needs, not future possibilities

## Common Patterns

### Pattern 1: Event Pub/Sub Integration
```python
# Publisher (Catalog context)
class ProductListing:
    def publish(self):
        self.status = PUBLISHED
        self.events.append(ProductPublished(
            listing_id=self.id,
            title=self.title
        ))

# Consumer (SearchIndex context)
@event_handler(ProductPublished)
def on_product_published(event):
    search_index.add_product(event.listing_id, event.title)
```

### Pattern 2: Aggregate Root Protection
```python
class Order:  # Aggregate root
    def __init__(self):
        self._items = []  # Private!
    
    def add_item(self, product_id, quantity):
        # All access through root
        item = OrderItem(product_id, quantity)
        self._items.append(item)
        self._recalculate_total()  # Maintains invariant
```

### Pattern 3: Anti-Corruption Layer
```python
# Protecting domain from external Stripe API
class StripeAdapter:
    def charge(self, payment: Payment):
        # Translate domain model to Stripe model
        stripe_charge = {
            'amount': payment.amount.cents,
            'currency': payment.amount.currency,
            'source': payment.payment_method.token
        }
        response = stripe.Charge.create(**stripe_charge)
        # Translate Stripe response back to domain
        return Payment Status(response.status)
```

## Troubleshooting

### Issue: Too Many Contexts
**Problem**: 10+ bounded contexts, integration nightmare

**Solution**: Merge related contexts, start with 3-5

### Issue: Anemic Domain Model
**Problem**: Entities with only getters/setters

**Solution**: Add behavior methods, move logic from services to entities

### Issue: Context Confusion
**Problem**: Unclear what belongs where

**Solution**: Use "owns" vs. "doesn't handle" boundaries explicitly

### Issue: Event Explosion
**Problem**: Too many fine-grained events

**Solution**: Focus on significant business events only

## Further Reading

- [Domain-Driven Design](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215) by Eric Evans
- [Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577) by Vaughn Vernon
- [Domain-Driven Design Distilled](https://www.amazon.com/Domain-Driven-Design-Distilled-Vaughn-Vernon/dp/0134434420) by Vaughn Vernon

---

**Generated by**: Spec Kit v0.0.23+  
**Command**: `/speckit.domain`  
**Documentation**: https://github.github.io/spec-kit/

