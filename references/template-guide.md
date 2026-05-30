# Template Guide — How to Fill Each Field

Detailed guidance for filling each of the 13 fields, with pass/fail examples in neutral, general-purpose domains (e-commerce, facility booking).

## Table of Contents
1. [Use Case ID](#1-use-case-id)
2. [Use Case Name](#2-use-case-name)
3. [Use Case History](#3-use-case-history)
4. [Actor](#4-actor)
5. [Description](#5-description)
6. [Preconditions](#6-preconditions)
7. [Postconditions](#7-postconditions)
8. [Priority](#8-priority)
9. [Frequency of Use](#9-frequency-of-use)
10. [Normal Course of Events](#10-normal-course-of-events)
11. [Alternative Courses](#11-alternative-courses)
12. [Exceptions](#12-exceptions)
13. [Includes](#13-includes)
14. [Special Requirements](#14-special-requirements)
15. [Assumptions](#15-assumptions)
16. [Notes and Issues](#16-notes-and-issues)

---

## 1. Use Case ID

**Purpose**: Unique identifier so requirements can be traced back to the UC.

**Rules**:
- Format: `UC-<module>-<sequence>` or `UC-X.Y` (hierarchical)
- Use a consistent naming convention across the project
- For related UC groups, use X.Y (e.g. UC-3.1, UC-3.2 belong to the "Order" group)
- Pad the sequence to 2-3 digits: `UC-ORDER-01`, `UC-ROOM-003`

**Good examples**:
- `UC-ORDER-01` (Order module, UC #1)
- `UC-ROOM-03` (Room booking module, UC #3)
- `UC-3.2` (hierarchical, 2nd sub-UC of group 3)

**Bad examples**:
- `UC1` (no scheme)
- `UseCase_PlaceOrder` (mixes name into ID — hard to maintain when name changes)

---

## 2. Use Case Name

**Purpose**: Short label describing the UC's goal.

**CRITICAL rules**:
- MUST follow **"Action verb + Object"** form
- 3-7 words, not too long
- DO NOT start with the actor name
- DO NOT use vague verbs ("manage", "handle", "process", "do")
- Reflects the actor's goal, not the implementation

**Pattern**: `<Verb> <Direct Object> [<modifier>]`

**Good examples**:
- ✅ "Place an order"
- ✅ "Book a meeting room"
- ✅ "Issue a refund receipt"
- ✅ "Approve a booking request"
- ✅ "Assign a license to a team member"

**Bad examples → how to fix**:
- ❌ "Order" → ✅ "Place an order"
- ❌ "Customer books room" (actor included) → ✅ "Book a meeting room"
- ❌ "Manage catalog" (vague verb) → split into "Create catalog item", "Update catalog item", "Archive catalog item"
- ❌ "Receipt is issued" (passive voice) → ✅ "Issue a refund receipt"

---

## 3. Use Case History

**Purpose**: Audit trail (Created By, Date Created, Last Updated By, Date Last Updated).

**Rules**:
- Created By: full name + role (e.g. "Jane Doe - BA Team")
- Date Created: YYYY-MM-DD format
- Last Updated By + Date Last Updated: update on every edit
- If unknown, use the placeholder `<TBD>` instead of leaving blank

---

## 4. Actor

**Purpose**: Identify who/what interacts with the system.

**Actor types**:
- **Primary actor**: Initiates the UC, benefits from the outcome
- **Secondary actor**: Supporting system/person (payment gateway, inventory service, calendar service)
- **Off-stage stakeholder**: Has interest but doesn't interact directly (regulators, auditors) — usually NOT listed in the Actor field

**Rules**:
- The primary actor MUST be a specific role/class — never write "User" generically
- A UC should have 1 primary actor (rarely 2+)
- If there's a secondary actor, label it clearly

**Good examples**:
- ✅ "Primary: Customer (registered account, email verified)"
- ✅ "Primary: Facility Manager (active manager account)"
- ✅ "Primary: Account Admin (organization owner with billing rights)"
- ✅ "Primary: Support Agent; Secondary: Fraud Check Service, Notification Service"

**Bad examples**:
- ❌ "User" (too generic)
- ❌ "Customer/Member" (ambiguous — are they the same role?)
- ❌ "System" (the system is the target of the UC, not an actor)

---

## 5. Description

**Purpose**: Summarize the UC in 2-3 sentences so readers grasp what it's about quickly.

**Rules — must answer 3 questions**:
1. **WHY**: The reason/trigger that leads to this UC
2. **WHAT**: What the actor does with the system
3. **OUTCOME**: The final result (new system state / value for the actor)

**Pattern**: `[When/To] <trigger/reason>, <actor> <action> in order to <outcome>.`

**Good example**:
> "When a customer wants a copy of a transaction record, the customer navigates to the order history and requests a receipt for a completed order. The UC ends when a receipt PDF is generated with a unique reference code, downloaded by the customer, and recorded in the Receipts table."

**Bad examples**:
> ❌ "This UC is about receipts." (too short, missing WHY and OUTCOME)
> ❌ "The receipt module has these steps: request, generate, download…" (describes flow, not a description)

---

## 6. Preconditions

**Purpose**: List conditions that MUST be true before the UC can start.

**CRITICAL rules**:
- Every precondition must be **verifiable** (boolean check)
- Number them: 1, 2, 3…
- Distinguish from Business Rules:
  - Precondition: checked BEFORE the UC starts
  - Business Rule: applied DURING the UC's flow
- Distinguish from Assumptions:
  - Precondition: REQUIRED for the UC to run
  - Assumption: BELIEVED to be true but not verified

**Good examples**:
```
1. Customer has logged in with a verified email address
2. The order status is 'Completed' (a receipt can only be issued for completed orders)
3. The order belongs to the logged-in customer
4. Receipt generation service is available
```

**Bad examples**:
- ❌ "System is operating" (too generic, not verifiable)
- ❌ "Customer wants a receipt" (motivation, not a condition)
- ❌ "Customer must have a valid payment method" (belongs in a payment UC, not a receipt UC)

---

## 7. Postconditions

**Purpose**: Describe the system state AFTER successful UC completion.

**Rules**:
- Verifiable (can be checked via DB query / API response)
- Cover all kinds of changes:
  - Data state (new record, status change)
  - User-facing state (notification sent, file available for download)
  - External system state (API call succeeded, calendar blocked)
- Number them

**Important**: A postcondition is a **state**, not an **action**.
- ✅ State: "Receipt record is saved with a unique reference code"
- ❌ Action: "System saves the receipt record" (this is a step in the Normal Course)

**Good example**:
```
1. Receipt record is created in the Receipts table with a unique reference code (format: RCPT-YYYY-NNNNN)
2. Receipt PDF is generated and stored in cloud storage, accessible via a permanent URL
3. Customer's order history displays a "Receipt available" badge for the order
4. Receipt verification page is accessible at verify.example.com using the unique code
5. A "Receipt ready" notification is sent to the customer's email and in-app notification center
```

---

## 8. Priority

**Purpose**: Define the implementation priority of the UC.

**Common schemes**:
- **MoSCoW**: Must / Should / Could / Won't
- **3-level**: High / Medium / Low

**Rules**:
- Use the SAME scheme as the project's SRS / PRD
- Justify (one sentence explaining why priority X)

**Good examples**:
- "High — Core feature; directly tied to conversion and retention metrics"
- "Medium — Enhances the admin experience; planned for phase 2"

---

## 9. Frequency of Use

**Purpose**: Estimate how often the UC will be executed → input for performance/capacity planning.

**Rules**:
- Use SPECIFIC NUMBERS (not "occasionally", "frequently")
- Suitable time units: per second, per hour, per day, per month
- If there are peak times, state them explicitly

**Good examples**:
- "~500 orders/day; peak ~100/hour during campaigns and flash sales"
- "~15 booking requests/day per manager; system-wide ~300/day across 20 locations; peak Monday mornings"

**Bad examples**:
- ❌ "Frequent"
- ❌ "Daily" (no volume)

---

## 10. Normal Course of Events

**Purpose**: Describe the happy path — steps from trigger to goal achieved.

**CRITICAL rules** (this is the most error-prone field):

### 10.1. Format
- Numbered list (1, 2, 3…)
- Each step: one single action
- Start with a clear subject (Actor / System)
- Active voice + present tense
- Short steps, 1-2 sentences each

### 10.2. Alternate Actor / System
Typical pattern: Actor → System → Actor → System…
- Odd steps: actor input
- Even steps: system response

### 10.3. DO NOT embed:
- ❌ If/else → move to Alternative Course
- ❌ Loops → use "Steps X-Y repeat until Z"
- ❌ Exceptions → move to Exceptions
- ❌ Internal system logic → that's design, not a UC

### 10.4. Start and end
- Step 1: Trigger (the event that activates the UC)
- Final step: Goal achieved (postcondition met)

**Good example (book a meeting room)**:
```
1. Member navigates to the "Rooms" section and selects a room.
2. System displays the room's details: capacity, amenities, and available time slots for the next 14 days.
3. Member selects a preferred date and time slot.
4. Member enters the meeting purpose (max 500 characters) and clicks "Send Request".
5. System validates that the member has at least 1 unused booking credit in their current plan.
6. System creates a booking request with status='Pending_Manager_Review' and notifies the facility manager.
7. System displays a confirmation screen: "Request sent! The facility manager will respond within 24 hours."
8. System invokes UC-NOTI-03 to send a confirmation email to the member.
```

**Bad examples → how to fix**:
- ❌ "1. If the member has a Premium plan, they can book any room; otherwise only shared desks…" → Move the branching to an Alternative Course
- ❌ "3. System validates. If invalid, show error. If valid, continue." → Validation-pass continues in flow; validation-fail goes into an Exception
- ❌ "5. System calls POST /api/v1/bookings with body {member_id, room_id, slot_id}" → Too technical. Say: "System creates the booking request in the booking system"

---

## 11. Alternative Courses

**Purpose**: A DIFFERENT path that still leads to the goal (still success), just a different route.

**Rules**:
- ID format: `UC-XX.AC.N` (AC = Alternative Course)
- Each AC starts with: "At step Y of the Normal Course, if [condition], execute the alternative: …"
- After the AC, state explicitly which step of the Normal Course to continue from

**Good example**:
```
UC-ORDER-01.AC.1: Pay using a gift card
At step 5 of the Normal Course, if the customer selects "Gift Card" as the payment method:
5a. System displays a gift card code input field.
5b. Customer enters the code and clicks "Apply".
5c. System validates the gift card (expiry, applicability, remaining balance).
5d. System updates the total amount to 0 → continue from step 7 of the Normal Course (no payment gateway call).
```

---

## 12. Exceptions

**Purpose**: Cases where the UC FAILS (goal is not achieved).

**Rules**:
- ID format: `UC-XX.EX.N` (EX = Exception)
- Each exception needs 3 parts:
  1. **Trigger condition**: When the exception occurs
  2. **System response**: What the system does
  3. **Final state**: The end state (rollback? partial? log?)

**Common failure modes to check** (don't forget):
- Validation errors (wrong format, missing field)
- Business rule violations (credit exceeded, item out of stock)
- External service failures (payment gateway timeout, inventory service unavailable)
- Network/connectivity issues
- Permission denied / authorization failure
- Concurrency conflict (last unit bought by another customer at the same time)
- Session timeout (manager idle too long on the detail view)

**Good example**:
```
UC-ORDER-01.EX.2: Item sells out between page load and checkout
Trigger: At step 7, the Inventory Service returns OUT_OF_STOCK because another customer bought the last unit milliseconds earlier.
Response: System displays "Sorry, this item just sold out. Add it to your wishlist to be notified when it is back."
Final state: Payment is refunded automatically within 1 business day. No order record is created. Customer is offered the back-in-stock waitlist.
```

---

## 13. Includes

**Purpose**: Reuse common functionality across UCs.

**Rules**:
- List sub-UCs "called" by this UC (UML «include» semantics)
- The sub-UC must exist (have its own spec)
- DO NOT use Includes just to group minor steps — only for logic reused in other UCs

**Good example**:
```
- UC-PAY-01: Process payment (called at step 6 of the Normal Course)
- UC-NOTI-01: Send order notification (called at step 9)
```

---

## 14. Special Requirements

**Purpose**: Non-functional requirements specific to this UC.

**Categories to cover**:
- **Performance**: Response time, throughput, concurrent users
- **Security**: Authentication, encryption, data privacy
- **Usability**: Accessibility, mobile-first requirements
- **Reliability**: Uptime, async fallback strategy
- **Compliance**: Regulatory requirements (tax invoicing, data retention)

**Rule**: DO NOT duplicate functional requirements — only list non-functional.

**Good example**:
```
- Performance: Product catalog page loads ≤ 2s under 5,000 concurrent users
- Security: Payment card data never stored on store servers; all card processing via a PCI-DSS certified gateway
- Reliability: If the inventory service is unavailable during checkout, payment must not be rolled back — retry asynchronously up to 30 min
- Compliance: Issue a tax invoice for transactions above the local statutory threshold, per applicable tax regulations
```

---

## 15. Assumptions

**Purpose**: Things assumed during analysis that haven't been verified.

**Difference vs Precondition**:
- Precondition: MUST BE TRUE, system can verify
- Assumption: BELIEVED TO BE TRUE, not required to verify

**Good example**:
```
1. Customer's email address is verified and active — confirmation emails will not bounce
2. Payment Gateway SLA is ≥ 99.5% uptime during business hours
3. Gift cards are pre-loaded by the operations team before distribution
4. Stock reservation completes synchronously in < 3s under normal load
```

---

## 16. Notes and Issues

**Purpose**: Open questions, TBDs, follow-up items.

**Format**:
```
[TBD-N] | Owner | Due Date | Resolution
```

**Good example**:
```
- [TBD-1] Should customers be able to gift an order to another account? | Owner: Product Team | Due: YYYY-MM-DD | Resolution: TBD — deferred to phase 2
- [TBD-2] What is the refund policy if a customer requests a refund within 7 days? | Owner: <TBD> | Due: YYYY-MM-DD | Resolution: TBD
- [NOTE] Confirmation email template must align with current brand guidelines — coordinate with Marketing team
```
