# Domain Command Fix - What Changed

## Problem Identified

The domain command was timing out because it was asking the AI to **generate 800-1000 lines of content from scratch in one operation**.

### Why It Failed:
1. Command said: "Do NOT run any scripts"
2. Command said: "Write the COMPLETE file at once (minimum 800-1000 lines)"
3. AI tried to generate everything → timeout → file write tool error

### Why Constitution Works:
1. ✅ Loads an EXISTING template with placeholders
2. ✅ Replaces placeholders incrementally  
3. ✅ Simple find/replace operations
4. ✅ Works section by section

---

## What Was Fixed

### Changed Approach: Generate → **Replace**

**Before (FAILED)**:
```
Step 1: Create directories manually
Step 2: Analyze domain
Step 3: Generate 800-1000 lines of content from scratch
Step 4: Write entire file at once
```

**After (WORKS)**:
```
Step 1: Run setup script (copies template)
Step 2: Load template (has placeholders like [PROJECT_NAME])
Step 3: Analyze domain description
Step 4: Replace placeholders INCREMENTALLY:
  - Phase 1: Basic info
  - Phase 2: Ubiquitous Language section
  - Phase 3: Bounded Contexts (one at a time)
  - Phase 4: Entities (one at a time)
  - Phase 5: Value Objects
  - Phase 6: Aggregates
  - Phase 7: Events
  - Phase 8: Integration
  - Phase 9: Implementation guidance
  - Phase 10: Remaining sections
Step 5: Update supplementary files
Step 6: Validate
Step 7: Report completion
```

---

## Key Changes

### 1. Uses the Setup Script
```markdown
### Step 1: Run Setup Script

Execute `{SCRIPT}` to initialize domain structure:
- Creates `.specify/domain/` directory
- Copies template from `.specify/templates/domain-model-template.md`
- Creates supplementary files (entities.md, events.md, context-map.md)
```

### 2. Works with Template Placeholders
```markdown
### Step 2: Load Domain Template

Load `.specify/domain/domain.md` and identify placeholders:
- [PROJECT_NAME]
- [CONTEXT_1_NAME], [CONTEXT_2_NAME]
- [ENTITY_1_NAME], [ENTITY_2_NAME]
- [EVENT_1_NAME], [EVENT_2_NAME]
- etc.
```

### 3. Incremental Replacement (10 Phases)
```markdown
### Step 4: Replace Placeholders Incrementally

DO NOT try to generate entire file at once. Work section by section:

Phase 1: Basic Information
Phase 2: Ubiquitous Language
Phase 3: Bounded Contexts
Phase 4: Domain Entities
Phase 5: Value Objects
Phase 6: Aggregates
Phase 7: Domain Events
Phase 8: Context Integration
Phase 9: Implementation Guidance
Phase 10: Remaining Sections

After each phase: Write updated domain.md back to disk
```

### 4. Explicit Instructions
```markdown
**Work Incrementally**:
- Run script first (Step 1)
- Load template (Step 2)
- Replace placeholders section by section (Steps 3-4)
- Write domain.md after each major phase
- Don't try to generate 800 lines at once
```

---

## Testing

### Updated File Location
The fixed command is now at:
```
test-domain-project/.cursor/commands/speckit.domain.md
```

### Test Command
In your Cursor session with test-domain-project:
```
/speckit.domain

e-commerce platform for handmade accessories
```

### Expected Behavior
1. ✅ Runs setup-domain.sh script
2. ✅ Creates .specify/domain/domain.md from template
3. ✅ Loads the template
4. ✅ Identifies placeholders like [PROJECT_NAME]
5. ✅ Replaces placeholders section by section
6. ✅ Writes file incrementally (no timeout)
7. ✅ Updates supplementary files
8. ✅ Reports completion

---

## Why This Fix Works

### Constitution Pattern
The domain command now follows the same pattern as constitution:

| Step | Constitution | Domain (Fixed) |
|------|-------------|----------------|
| 1 | Load `/memory/constitution.md` | Run script to copy template |
| 2 | Identify `[PLACEHOLDERS]` | Load domain.md, find `[PLACEHOLDERS]` |
| 3 | Replace incrementally | Replace incrementally (10 phases) |
| 4 | Write back | Write back after each phase |
| 5 | Validate | Validate completeness |
| 6 | Report | Report with summary |

### Key Insight
AI agents work best with:
- ✅ **Structured templates** with clear placeholders
- ✅ **Incremental updates** (section by section)
- ✅ **Small write operations** (not 800+ lines at once)
- ✅ **Clear phases** with intermediate saves

NOT with:
- ❌ Generate massive content from scratch
- ❌ Single giant write operation
- ❌ Complex multi-step reasoning + writing

---

## Summary

**Problem**: Command asked AI to generate 800-1000 lines at once → timeout

**Solution**: Work like constitution - copy template, replace placeholders incrementally

**Result**: Should now work without timeouts! 🎉

---

**Test it now and let me know if it works!**

