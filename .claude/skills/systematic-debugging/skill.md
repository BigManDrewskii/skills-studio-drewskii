# Systematic Debugging

Structured debugging workflow. Prevents shotgun debugging by forcing you to understand the problem before proposing solutions.

## Usage

```
/systematic-debugging [describe your bug, error, or unexpected behavior]
```

## The Debugging Trap

Most debugging looks like this:
1. See error
2. Assume cause
3. Change code
4. Hope it works
5. Repeat 10 times
6. It finally works, nobody knows why

This is **shotgun debugging**. It's wasteful, unreliable, and doesn't build understanding.

## The Systematic Approach

Debugging = problem solving

Follow this sequence:

```
1. OBSERVE   - What actually happened?
2. DEFINE    - What SHOULD happen?
3. ISOLATE   - Where's the gap?
4. HYPOTHESIZE - What could cause this?
5. TEST      - Verify each hypothesis
6. CONFIRM   - Fix + verify root cause addressed
```

## Step-by-Step Framework

### 1. OBSERVE (Gather Facts)

Before touching any code:

- **What is the exact error message?** (Copy it verbatim)
- **When does it happen?** (Every time? Intermittent? Specific conditions?)
- **What changed recently?** (New code, dependency update, config change?)
- **What's the actual vs expected output?**

**DO NOT:** Assume you know the cause
**DO:** Collect evidence like a crime scene

### 2. DEFINE (The Problem Statement)

Use this template:

```
Problem: [1 sentence description]
Context: [When it happens]
Expected: [What should happen]
Actual: [What actually happens]
Impact: [Why it matters]
```

If you can't fill this out, you don't understand the problem yet.

### 3. ISOLATE (Find the Scope)

Narrow down WHERE the problem occurs:

- **Minimal reproduction**: Can you reproduce it in 5 lines of code?
- **Binary search**: Comment out half the code. Still broken? Yes → it's in that half.
- **Boundary testing**: What's the simplest case that breaks? Most complex that works?

Your goal: Find the smallest code change that makes it fail.

### 4. HYPOTHESIZE (Generate Explanations)

Now (and only now) brainstorm causes:

**Requirement:** Generate at least 3 hypotheses

```
H1: [Most likely cause]
H2: [Second most likely]
H3: [Unusual but possible]
```

For each hypothesis:
- What evidence supports it?
- What evidence would rule it out?

**Rule:** If you can't think of 3 things that might be wrong, you don't understand the problem yet. (This is straight from "Are Your Lights On?")

### 5. TEST (Verify Each Hypothesis)

Design tests that can PROVE or DISPROVE each hypothesis:

```
If H1 is true, then [test] should show [result]
If H2 is true, then [test] should show [result]
```

Run the tests. Eliminate hypotheses.

**Key:** Each test should eliminate possibilities, not just confirm your hunch.

### 6. CONFIRM (Fix + Verify)

When you find the root cause:

1. **Fix it** (smallest change that addresses root cause)
2. **Verify the fix** (original issue resolved)
3. **Check for side effects** (did you break anything else?)
4. **Add a test** (ensure this can't happen again)

## Examples

### Example 1: API Returns 500 Error

**Bad (shotgun):**
```
"Probably the database is down. Let me restart it. No, still broken. Maybe the API key? Let me regenerate it. Still broken. Maybe the endpoint changed?"
```

**Good (systematic):**
```
OBSERVE:
- Exact error: "500 Internal Server Error" on POST /api/users
- Happens: Every time, but only for this endpoint
- Recent change: Added new field to user schema 2 hours ago
- Actual: 500 error
- Expected: 201 Created

DEFINE:
Problem: User creation endpoint crashing
Context: All requests to POST /api/users fail with 500
Expected: Returns 201 and creates user
Actual: Returns 500
Impact: Can't create new users

ISOLATE:
- Other endpoints work? Yes (GET /api/users returns 200)
- Simple request fails? Yes (even with minimal payload)
- Regression? Code worked before schema change

HYPOTHESIZE:
H1: New schema field breaks validation (most likely)
H2: Database migration incomplete (second)
H3: Database connection pool exhausted (unlikely, other endpoints work)

TEST:
If H1: Check server logs for validation error
Result: Found "TypeError: Cannot read property 'validate' of undefined"
→ H1 confirmed

FIX:
Added validation for new field. Verified.
Added test case for this field.
```

### Example 2: UI Component Not Rendering

**Bad (shotgun):**
```
"Must be a CSS issue. Let me adjust z-index. No. Maybe display: none? No. Let me rewrite the whole component."
```

**Good (systematic):**
```
OBSERVE:
- Component renders in Storybook but not in app
- No console errors
- React DevTools shows component in tree (but empty)

DEFINE:
Problem: UserProfileAvatar appears but doesn't display content
Context: Works in isolation, broken in app context
Expected: Shows user avatar image
Actual: Renders empty div

ISOLATE:
- Props passed correctly? Yes (checked in DevTools)
- CSS loaded? Yes (other styles work)
- Data fetch successful? Yes (image URL present)

HYPOTHESIZE:
H1: CSS conflict from parent component
H2: Image URL invalid but not throwing error
H3: Conditional rendering logic wrong

TEST:
If H1: Remove parent styles → still broken
If H2: Hardcode known-good URL → works
If H3: Check rendering logic → found bug: `if (!user?.avatar?.url) return null`

FIX:
User object had `avatar` but `url` was undefined string "", not falsy.
Changed condition to `if (!user?.avatar?.url || url === '') return null'
```

## Anti-Patterns to Avoid

| ❌ Shotgun Debugging | ✅ Systematic Debugging |
|---------------------|------------------------|
| "Let me just change this" | "What evidence do I have?" |
| Try 5 things at once | Test one hypothesis at a time |
| "That's weird" (move on) | "That's weird" (investigate) |
| Fix symptoms, not root cause | Trace to root cause |
| "It works, don't know why" | "It works, here's why" |
| Comment out code randomly | Binary search to isolate |

## When to Use This Skill

Use systematic-debugging when:
- You encounter ANY bug or error
- Tests fail and you don't know why
- User reports unexpected behavior
- You're about to make "just a quick fix"
- You catch yourself saying "let me try..."

**Red flag:** If you've made 3+ changes without understanding the root cause, you're shotgun debugging. STOP. Use this workflow.

## Quick Reference Card

```
□ OBSERVE: Error message, timing, context, actual vs expected
□ DEFINE: Write problem statement using template
□ ISOLATE: Find minimal reproduction, narrow scope
□ HYPOTHESIZE: Generate 3+ possible causes
□ TEST: Design experiments to eliminate hypotheses
□ CONFIRM: Fix root cause, verify, add test
```

## Philosophy

This approach is grounded in the same principles as "Are Your Lights On?":

- **A problem = difference between desired and perceived**
- **Never be sure you have the correct definition** (even after solving)
- **Every solution creates the next problem** (fix mindfully)
- **If you can't think of 3 flaws in your understanding, you don't understand**

Debugging isn't about fixing code. It's about understanding systems.

## Advanced: When You're Stuck

After 3 failed hypotheses:

1. **STOP making changes**
2. **Revert to last known working state**
3. **Document what you've tried**
4. **Ask for help** (bring your systematic documentation)

The person helping you will ask for this information anyway. Having it ready makes you look competent and gets you better help faster.
