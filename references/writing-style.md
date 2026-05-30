# Writing Style Guide

Compiled from Alistair Cockburn ("Writing Effective Use Cases") + IIBA BABOK + common practice across general software domains.

## Supreme principle: READABILITY FIRST

Cockburn's famous quote: "Write clearly. Readability is the most important thing."

A good UC is one where:
- Non-technical stakeholders can grasp the meaning
- Developers have enough to code from
- QA has enough to write test cases
- A new BA can update it when things change

---

## Rule 1: Active Voice + Present Tense

### Active voice
Use active sentences where the subject performs the action.

- ✅ "Customer clicks the **Checkout** button"
- ❌ "The **Checkout** button is clicked by the customer"

- ✅ "System saves the order record to the database"
- ❌ "The order record is saved to the database by the system"

### Present tense
Use simple present tense, avoid future/past.

- ✅ "System displays the order confirmation screen"
- ❌ "System will display the order confirmation screen"
- ❌ "System displayed the order confirmation screen"

---

## Rule 2: Clear subject — Subject + Verb + Object

Every step must start with a **specific subject**: an actor name or "System".

- ✅ "Member selects a preferred room time slot"
- ❌ "Selects a preferred time slot" (no subject)

- ✅ "System validates the member's remaining booking credit"
- ❌ "Validates the remaining credit" (passive, unclear who's doing it)

---

## Rule 3: One step = one action

Each step in the Normal Course does exactly one thing. If you see "and" connecting different kinds of action → split the step.

- ✅ "3. Member enters the room, preferred slot, and meeting purpose." (same kind — filling a form)
- ❌ "3. Member fills in the meeting purpose and clicks Confirm." → split into 2 steps:
  - "3. Member enters the meeting purpose (max 500 characters)."
  - "4. Member clicks the **Send Request** button."

**Why**: "Clicking Send Request" usually triggers system validation → it needs to be a separate step so an Exception "credit exceeded" can be attached to it.

---

## Rule 4: Avoid vague verbs

Vague verbs = verbs that don't convey a specific action.

| ❌ Vague | ✅ Specific |
|---------|------------|
| Manage | Create / Update / Archive / View |
| Handle | Validate / Process / Reject / Escalate |
| Do | Submit / Approve / Assign / Generate |
| Make | Issue / Build / Render / Compute |
| Get | Retrieve / Fetch / Query / Download |
| Use | Apply / Invoke / Execute / Redeem |
| Take care of | Specific verb |

**Apply this to both the UC Name and the step text.**

---

## Rule 5: Avoid implementation details

A UC describes **WHAT** (the action), not **HOW** (the mechanism). Leave HOW for the design phase.

- ❌ "System calls POST /api/v1/orders with header Authorization Bearer {token}, body {product_id, customer_id}…"
- ✅ "System creates the order record in the order system"

- ❌ "System inserts a row into the tbl_orders table with fields: order_id, product_id, customer_id, created_at…"
- ✅ "System saves the order to the database"

- ❌ "System renders the <OrderSuccessModal> React component with prop productTitle='Wireless Mouse'…"
- ✅ "System displays the Order Confirmed screen with the product name and order number"

**Exception**: If the UC is specifically an integration spec, it can be more detailed — but still use business language.

---

## Rule 6: Consistent numbering

### Normal Course
Numbered list starting at 1.

### Alternative Course
Sub-numbering by original step + letter:
- AC at step 5 → step 5a, 5b, 5c
- After the AC, state "continue from step N of the Normal Course"

### Exception ID
Format: `UC-XX.EX.N` (numbered independently, not tied to a step)

### Pre/Postconditions
Numbered list starting at 1.

---

## Rule 7: Naming UI elements

When mentioning a UI element in a step, use bold and the actual on-screen label:

- ✅ "Customer clicks the **Checkout** button"
- ✅ "System displays the **Order Summary** screen"
- ✅ "Customer selects **Gift Card** from the payment method dropdown"

Reason: Easy to trace back to wireframes/mockups during design handoff.

---

## Rule 8: Avoid vague words

| ❌ Vague | ✅ Specific |
|---------|------------|
| In some cases | When condition X occurs |
| May / can | When [condition], system [action] |
| Sometimes | X% of the time / Y times per Z |
| If needed | When [specific trigger] |
| Valid | Meets the criteria: … (list them) |
| Appropriate | Per project policy [reference] |
| Quickly | Within X seconds |
| User | Customer / Member / Facility Manager / Account Admin |

---

## Rule 9: Don't embed business rules in steps

A Normal Course step describes **flow**. Business rules (validation rules, limits, business logic) should:
- Reference Special Requirements by rule ID
- Or live in a separate Business Rule document (BR-XX-YY)

- ❌ "5. System validates: meeting purpose must be ≤ 500 chars, member must have ≥ 1 unused credit, slot must be ≥ 2 hours in the future, room must not be under maintenance…"
- ✅ "5. System validates the booking request according to business rule BR-ROOM-001."
  - (Then list BR-ROOM-001 in Special Requirements or a separate BR document)

---

## Rule 10: Length guidelines

- **UC Name**: 3-7 words
- **Description**: 2-4 sentences, ~50-100 words
- **Normal Course**: 5-15 steps (usually 7-10)
- **Each step**: 1 sentence, max 2 sentences, < 30 words
- **Alternative Courses**: 1-5 ACs per UC (more → consider splitting the UC)
- **Exceptions**: 3-7 for a typical UC
- **Total UC document**: 2-5 A4 pages

If you exceed the guideline:
- UC too long → split via Includes
- Too many ACs/EXs → review the scope, the UC might be carrying too much

---

## Rule 11: Consistency across the project

Be consistent across the whole document set:
- Actor names (don't switch between "Customer", "Buyer", "User", "Shopper")
- System component names (Inventory Service, Stock Service, Warehouse API → pick one)
- Screen/menu names (must match the wireframe or product spec)
- Naming convention for UC IDs

Tip: Maintain a **Glossary** at the front of the document set. Agree upfront: is it "Customer" or "Buyer"? "Member" or "Subscriber"?

---

## Rule 12: Internationalization

If the platform has i18n requirements:
- Screen/button names in the UC can use keys instead of hard-coded text
- E.g. replace "clicks the **Checkout** button" with "clicks the {btn.checkout} button"
- For most UC specs, plain English labels are fine

---

## Anti-patterns — the 10 most common mistakes

### 1. UC is a pixel-by-pixel UI spec
❌ "System displays a modal with a blue #1E88E5 header 'Order Confirmed', a checkmark icon, and a product thumbnail on the left..."
→ That's a wireframe annotation. A UC says: "System displays the Order Confirmed screen with the product name and an order number."

### 2. Mixing actor and system in one step
❌ "3. Member selects the slot and system validates credit."
→ Split into 2 steps.

### 3. Skipping system response
❌ "1. Customer clicks Checkout. 2. Customer enters payment info. 3. Customer confirms."
→ System responses between steps are missing. A UC must show the DIALOG actor ↔ system.

### 4. Embedded conditional logic
❌ "5. If the member has a Premium plan, system allows any room; otherwise only shared desks are shown."
→ Split into Normal Course (default case) + AC (Premium path) or Exception (unauthorized access).

### 5. Vague trigger
❌ "When the customer wants a receipt, they..."
→ Be specific: "When the customer navigates to the **Order History** tab and selects a completed order..."

### 6. Postcondition is an action instead of a state
❌ "System sends a receipt to the customer" (action)
→ "A receipt email has been delivered to the customer's registered email address" (state) ← verifiable

### 7. UC with 2 primary actors
❌ Primary: Customer + Account Admin (both initiating the UC)
→ Split into 2 UCs: one for self-service, one for admin-assigned.

### 8. Vague "System processes"
❌ "5. System processes the order."
→ Be specific: "System creates the order record and reserves stock in the Inventory Service."

### 9. Repeating the Description in the Normal Course
If the Description already states the full flow, don't copy it into the Normal Course. The Description is a 2-3 sentence summary; the Normal Course is the detailed step-by-step.

### 10. Forgetting failure modes
A UC with only a Normal Course + 1 generic "error" Exception → not enough.
Always cover the relevant failure modes: payment failures, quota/credit exhaustion, external service timeouts, concurrency conflicts (two actors grabbing the last slot/unit), and permission/role mismatches.
