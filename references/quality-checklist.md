# Quality Checklist — 20 Points to Validate a Use Case

Run this checklist BEFORE handing over a UC. Each item has: definition, how to check, pass/fail examples.

## How to use

1. After writing the UC, walk through items C1-C20
2. Mark Status: ✅ Pass / ❌ Fail / ⚠️ Needs review
3. If Fail → fix it or flag it to the user
4. Output a summary table at the end

```
| Item | Status | Note |
| C1   | ✅     | UC Name "Place an order" follows the format |
| C2   | ⚠️     | UC could be split further — confirm with PO |
| ...  | ...    | ... |
```

---

## GROUP A: Scope & Identification (C1-C5)

### C1. UC Name follows "verb + object", active voice
**Definition**: UC Name starts with an active verb + an object noun, with no actor name embedded.

**How to check**: Parse the UC Name → identify the leading verb → verify it's an action verb.

**Pass**: "Place an order", "Approve a booking request", "Issue a refund receipt"
**Fail**: "Order" (no verb), "Customer books room" (actor included), "Manage catalog" (vague verb)

---

### C2. UC is at user-goal level (passes the coffee-break test)
**Definition**: After completing the UC, the actor can stop and take a break — the goal is achieved.

**How to check**: Read the Postconditions → ask yourself "Is this a business-meaningful result?"
- If the result is just a sub-step (e.g. "OTP is verified") → UC is too small
- If the result spans multiple sessions → UC is too large

**Pass**:
- "Place an order" → postcondition: order confirmed, stock reserved
- "Book a meeting room" → postcondition: booking request submitted, manager notified

**Fail**:
- "Verify OTP" (too small — just a sub-step of another UC) → should be an Includes
- "Manage the entire order lifecycle" (too large — spans many sessions) → split into many UCs

---

### C3. UC ID is unique and follows naming convention
**Definition**: ID is unique in the project and matches the standard format.

**How to check**:
- Check the master UC list — is the ID unique?
- Does the format match `UC-<module>-<seq>`?

**Pass**: "UC-ORDER-01" (unique, correct format), "UC-ROOM-03"
**Fail**: "UC1" (no module), "UseCase_PlaceOrder" (name embedded)

---

### C4. Exactly 1 primary actor + clear business goal
**Definition**: One UC has 1 primary actor (the initiator) and 1 specific goal.

**How to check**:
- Actor field → is there "Primary: [X]"?
- Description → does it state the goal clearly?
- If you see 2 primary actors → flag for splitting

**Pass**: Primary: Customer. Goal: place an order and receive confirmation with stock reserved.
**Fail**: Primary: Customer + Account Admin (2 actors). → Split: "Customer self-orders" and "Admin places order on behalf of a member" as 2 UCs.

---

### C5. System boundary is clear
**Definition**: The UC describes interaction with one specific system, not multiple systems mixed together.

**How to check**: Read the Normal Course → do the "System..." steps consistently refer to one system?

**Pass**: All steps refer to "the store platform". Inventory Service and Payment Gateway are secondary actors.
**Fail**: Mixing the store web platform + mobile app + third-party warehouse API as if they were one system. → Split by system boundary or clarify the primary system.

---

## GROUP B: Actor & Context (C6-C8)

### C6. Actor is a specific role/class
**How to check**: Is the actor a specific role/class rather than "User"?

**Pass**: "Customer (registered account)", "Facility Manager (active manager account)"
**Fail**: "User", "Person", "Actor 1"

---

### C7. Description answers WHY + WHAT + OUTCOME
**How to check**: Read the Description → check that all 3 elements are present.

**Pass**:
"When a customer wants a copy of a transaction record [WHY], the customer navigates to the order history and requests a receipt [WHAT]. The UC ends when a receipt PDF is generated with a unique reference code and emailed to the customer [OUTCOME]."

**Fail**: "This UC is about issuing receipts." (missing WHY and OUTCOME)

---

### C8. Frequency of Use is quantified
**How to check**: Does the Frequency field contain a NUMBER?

**Pass**: "~500 orders/day store-wide; peak ~100/hour during promotional campaigns"
**Fail**: "Frequent", "Often during sales" (no volume)

⚠️ Acceptable: "TBD — awaiting analytics data from ops team" + logged in Notes as [TBD-N]

---

## GROUP C: Pre/Post Conditions (C9-C11)

### C9. Preconditions are verifiable
**How to check**: Can each precondition be verified by a query / boolean test?

**Pass**: "Order status is 'Completed' (a receipt can only be issued for completed orders)" (DB query), "Payment Gateway is available" (health check)
**Fail**: "Customer is motivated to buy" (motivation — not verifiable), "System is ready" (too vague)

---

### C10. Postconditions cover the success state + all changes
**How to check**: Do the postconditions describe all changes after the UC runs?
- Data changes (which records, which fields)
- External state (notification sent, calendar blocked, file generated)
- User-visible state (new screen, badge unlocked)

**Pass**:
```
1. Order record created with status='Confirmed'
2. Stock reserved for each ordered item
3. Payment transaction saved with status='Completed'
4. Confirmation email + in-app notification sent within 60s
5. Cart is cleared
```

**Fail**: Only "Order succeeded" → missing all state detail.

---

### C11. Preconditions are not confused with Assumptions
**How to check**: Distinguish:
- Precondition: MUST BE TRUE, system can check
- Assumption: BELIEVED to be true, not verified

**Common mistake**: Putting "Customer has basic computer literacy" in Precondition → WRONG, this is an Assumption. The system cannot check it.

---

## GROUP D: Normal Course (C12-C15)

### C12. Numbered list, one action per step
**How to check**: Does each step:
- Start with a number (1., 2., 3...)
- Contain only one main action
- Avoid "and" connecting two different-kind actions

**Pass**: "3. Customer enters the shipping address, preferred date, and delivery note." (same kind — input fields)
**Fail**: "3. Customer enters the address and clicks Checkout and waits for confirmation." (3 actions in one step)

---

### C13. Alternates Actor / System with clear subjects
**How to check**: Read the steps — is there an alternating Actor/System pattern?

**Pass**:
```
1. Customer clicks Checkout               ← Actor
2. System displays the Order Summary      ← System
3. Customer selects a payment method      ← Actor
4. Customer clicks Proceed to Payment     ← Actor
5. System invokes the Payment Gateway     ← System
```
(OK to have 2 consecutive Actor steps when both are input — still clear)

**Fail**: Only "Customer does X, then Y, then Z" with no system response anywhere.

---

### C14. NO embedded if/else/loop in the Normal Course
**How to check**: Search the Normal Course for "if", "in case", "otherwise" → flag.

**Pass**:
```
5. System validates the member's remaining booking credit.
6. System creates the booking request with status='Pending_Manager_Review'.
```

**Fail**:
```
5. If the member has a Premium plan, system shows all rooms; if Basic, system shows only shared desks; if credit is 0, system blocks the action.
```
→ Split into: Normal Course (default Premium flow) + AC (Basic tier) + Exception (credit exhausted).

---

### C15. Flow runs from trigger to postcondition
**How to check**:
- Does step 1 match the trigger in the Description?
- Does the final step achieve the postcondition?
- Are there any "dangling" steps?

**Pass**: Step 1 "Customer clicks Checkout" (trigger) → step 9 "System sends confirmation notification" (postcondition achieved).
**Fail**: Final step is "System saves the order" but postcondition says "Confirmation notification is sent" → flow is incomplete.

---

## GROUP E: Alternative & Exception (C16-C18)

### C16. Each AC specifies "at step N" + condition
**How to check**: Does each Alternative Course have:
- ID format `UC-XX.AC.N`
- Opening sentence: "At step Y of the Normal Course, if [condition]..."
- Sub-steps numbered 5a, 5b...
- Closing sentence: "continue from step Z of the Normal Course"

**Pass**:
```
UC-ORDER-01.AC.1: Pay using a gift card
At step 5 of the Normal Course, if the customer selects Gift Card:
5a. System displays a gift card code field.
5b. Customer enters the code and clicks Apply.
5c. System validates the gift card → continue from step 7 of the Normal Course.
```

**Fail**:
```
AC1: If the customer has a gift card, they can use it instead of paying.
```
(Too vague, no step reference, no sub-steps, no rejoining instruction)

---

### C17. Each Exception has trigger + response + final state
**How to check**: Does each exception have all 3 parts?

**Pass**:
```
UC-ORDER-01.EX.2: Item sells out mid-flow
Trigger: At step 7, the Inventory Service returns OUT_OF_STOCK.
Response: System displays "This item just sold out. Join the back-in-stock waitlist."
Final state: Payment refunded within 1 business day. No order created. Waitlist offer shown.
```

**Fail**:
```
EX1: If an error occurs, system shows an error message.
```
(Vague — no trigger, no response detail, no final state)

---

### C18. Common failure modes are covered
**How to check**: Does the UC cover at least the common failure types relevant to the feature?

| Failure type | Required? |
|---|---|
| Validation error (invalid input) | ✅ |
| Business rule violation (credit exceeded, item out of stock) | ✅ |
| External service failure (payment gateway, inventory, calendar timeout) | ✅ |
| Authentication/Authorization failure | ✅ if UC has auth |
| Network/connectivity issue | ✅ for mobile flows |
| Concurrency conflict (two actors grabbing the last slot/unit) | ✅ for order/booking UCs |
| Session timeout (manager idle on detail view) | ✅ for UCs with long review flows |

**Tip**: If the UC has only 1-2 Exceptions → suspicious. Order and booking UCs typically need 3-5.

---

## GROUP F: Completeness (C19-C20)

### C19. Includes (if any) point to existing UCs
**How to check**: Does each UC in the Includes field have a valid ID + does that UC actually exist?

**Pass**: "Includes: UC-PAY-01 (Process payment)" → UC-PAY-01 has been written and is in the UC register.
**Fail**: "Includes: Payment UC" → ID not specific, or referenced UC doesn't exist yet.

---

### C20. Special Requirements don't duplicate functional requirements
**How to check**: Is each item in Special Requirements a non-functional requirement?

**Pass** (non-functional):
- "Product catalog loads ≤ 2s under 5,000 concurrent users"
- "Audit log retained for 3 years"
- "Comply with applicable tax-invoicing regulations"

**Fail** (functional — belongs in Normal Course / Business Rule):
- "Validate that the gift card code is 16 characters" → validation logic, belongs in a Normal Course step or BR
- "Customer can only place 10 orders per day" → business rule, not a Special Requirement

---

## Validation Report

After checking all 20 items, output the report in this format:

```markdown
## Validation Result for UC-ORDER-01

| # | Item | Status | Note |
|---|------|--------|------|
| C1 | UC Name format | ✅ | "Place an order" — active verb + object |
| C2 | User-goal level | ✅ | Passes coffee-break test |
| C3 | UC ID unique | ✅ | Follows convention |
| C4 | 1 primary actor | ✅ | Customer |
| C5 | System boundary | ✅ | Store platform |
| C6 | Specific actor | ✅ | |
| C7 | Description WHY+WHAT+OUTCOME | ✅ | |
| C8 | Frequency quantified | ⚠️ | TBD — awaiting analytics from ops team |
| C9 | Preconditions verifiable | ✅ | 4/4 verifiable |
| C10 | Postconditions cover state | ✅ | 5 postconditions |
| C11 | No Pre/Assumption mix | ✅ | |
| C12 | Numbered, 1 action/step | ✅ | 9 steps |
| C13 | Actor/System alternating | ✅ | |
| C14 | No nested if/else | ✅ | |
| C15 | Flow complete | ✅ | |
| C16 | AC has "at step N" | ✅ | 2 ACs, all properly anchored |
| C17 | Exception has 3 parts | ✅ | 4 exceptions, all complete |
| C18 | Common failure modes covered | ✅ | Payment fail, out of stock, inventory unavailable — all covered |
| C19 | Includes valid | ✅ | UC-PAY-01, UC-NOTI-01 |
| C20 | Special Req non-functional | ✅ | |

**Summary**: 19/20 ✅ + 1 ⚠️. UC is ready for stakeholder review.
**Follow-up**: C8 — Frequency of Use awaiting analytics data from the ops team [TBD-3].
```
