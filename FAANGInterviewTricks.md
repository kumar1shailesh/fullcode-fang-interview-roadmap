# 🎯 FAANG Interview Tricks — recognition, behaviour, and rescue

Two things win coding interviews: **recognising the pattern fast**, and
**behaving like someone your interviewer wants as a teammate**. This file trains
both.

---

## 1. The Pattern Recognition Table (memorise this)

When you read a problem, match its wording to a row. This is the single most
valuable table in the repo.

| If the problem says... | The pattern is probably... | Template file |
|------------------------|----------------------------|---------------|
| "sorted array" + "find a pair / two elements" | **Two Pointers** | `01` |
| "reverse", "palindrome", "move X to the end" | **Two Pointers** | `01` |
| "longest/shortest **subarray** or **substring**" | **Sliding Window** | `02` |
| "maximum sum of k consecutive", "at most k" | **Sliding Window** | `02` |
| "have I seen this before?", "count occurrences", "anagram" | **HashMap / Set** | `03` |
| "sum of range", "product except self", "running total" | **Prefix Sum** | `04` |
| "matching brackets", "undo", "most recent", "next greater" | **Stack (Monotonic)** | `05` |
| "reverse a list", "nth from end", "cycle", "middle node" | **Linked List + Pointers** | `06` |
| "sorted" + "find target / minimum / first true" | **Binary Search** | `07` |
| "minimum capacity/speed/days such that..." | **Binary Search on Answer** | `07` |
| "tree", "levels", "depth", "path", "ancestor" | **DFS / BFS on Tree** | `08` |
| "grid", "islands", "shortest path", "connected", "rotting" | **DFS / BFS on Graph** | `09` |
| "top K", "K largest/smallest", "K closest", "median" | **Heap** | `10` |
| "all combinations / permutations / subsets", "generate every" | **Backtracking** | `11` |
| "min/max ways", "can you reach", "optimal", overlapping subproblems | **Dynamic Programming** | `12` |
| "merge overlapping", "meeting rooms", "min arrows/removals" | **Intervals (sort first)** | `13` |
| "prefix", "autocomplete", "starts with", "dictionary of words" | **Trie** | `14` |

> **When stuck, ask:** *"What's the smallest sub-answer I could reuse?"* If a
> big answer is built from smaller answers of the same shape → recursion / DP.

---

## 2. The 30-second problem triage

Before writing any code, spend 30 seconds:

```
1. INPUT     — array? tree? graph? string? What size? (n≤20 hints at O(2ⁿ)!)
2. OUTPUT    — one number? a boolean? all solutions? the count?
3. CONSTRAINT— sorted? distinct? positive only? (each is a hint)
4. PATTERN   — match against the table above.
5. COMPLEXITY— state your target out loud.
```

**Constraint → complexity cheat (interviewers plant these):**

| Constraint on n | Likely intended complexity |
|-----------------|----------------------------|
| n ≤ 10–12 | O(n!) — permutations / backtracking |
| n ≤ 20–25 | O(2ⁿ) — subsets / bitmask |
| n ≤ 500 | O(n³) |
| n ≤ 5,000 | O(n²) |
| n ≤ 10⁶ | O(n) or O(n log n) — no nested loops! |
| n ≥ 10⁹ | O(log n) — binary search or math |

---

## 3. The interview script (what to actually say)

**Minute 0–2 — Clarify (never skip):**
> "Just to confirm — the array can contain negatives? Can it be empty? Are there
> duplicates? Do you want any valid answer or all of them?"

This buys thinking time *and* shows maturity. Interviewers routinely give hints
here.

**Minute 2–5 — Plan out loud:**
> "The brute force is nested loops, O(n²). But since we only need to know if we've
> seen the complement, I can use a hashmap for O(1) lookups → O(n) total, O(n)
> space. I'll code that."

**Minute 5–20 — Code while narrating:**
> "I'll iterate once. For each number I check if `target - num` is already in my
> map. If yes, return both indices. If not, store this number."

**Minute 20–25 — Test on paper:**
> "Let me trace `[3,2,4]`, target 6. i=0: need 3, map empty, store {3:0}. i=1:
> need 4, not in map, store {3:0, 2:1}. i=2: need 2, it's in map! return [1,2]. ✓"

**If asked to optimise:**
> "For O(1) space I'd sort and two-pointer, but that costs O(n log n) time and
> loses original indices. The hashmap is the better trade-off here unless space
> is critical."

---

## 4. When you're stuck (the rescue ladder)

Climb these in order. Say each out loud — a good "stuck" narration still scores.

1. **Smaller example.** Solve n=1, n=2, n=3 by hand. The pattern often appears.
2. **Brute force fully.** A working O(n²) beats a broken O(n). Code it, then optimise.
3. **What am I recomputing?** Repeated work → cache it (hashmap / DP).
4. **Sort it.** If order doesn't matter, sorting often unlocks two-pointer or greedy.
5. **Reverse the question.** "Longest valid" ↔ "shortest invalid". "Max" ↔ "min".
6. **Pick a data structure.** Need order? heap. Need seen-before? set. Need next-greater? stack.
7. **Ask for a hint.** "I'm considering a hashmap here — am I on the right track?"
   Asking well is a *positive* signal, not a failure.

---

## 5. Bug-avoidance checklist (the top 7 interview bugs)

Before you say "done", check:

- [ ] **Off-by-one:** is it `range(n)` or `range(n-1)`? `<` or `<=`?
- [ ] **Empty input:** does `arr[0]` crash on `[]`?
- [ ] **Single element:** does a two-pointer loop even start?
- [ ] **Integer overflow / mid:** use `mid = lo + (hi-lo)//2` (habit for other languages).
- [ ] **Mutating while iterating:** don't add/remove from a list you're looping.
- [ ] **Return vs print:** return the value; don't just print it.
- [ ] **All-same / all-negative:** does your logic assume distinct or positive?

---

## 6. Behavioural signals that get you the offer

Coding correctness is ~60%. The rest:

- **Think out loud** — silence reads as "stuck" even when you're not.
- **Take hints gracefully** — "Oh, that's a great point, so if I..." not defensiveness.
- **Admit uncertainty honestly** — "I'm not 100% sure this handles duplicates,
  let me check" beats false confidence.
- **Manage time** — if 10 min left and no solution, say "let me get the brute
  force working end-to-end first."
- **Be someone they'd debug a prod issue with at 2am.** That's the real bar.

---

## 7. The night-before and day-of

**Night before:** don't grind new problems. Re-read this file + `cheatsheet.py`.
Sleep. A rested brain recognises patterns; a tired one blanks.

**Day of:**
- Re-derive 2 templates from memory as a warm-up (e.g. binary search, BFS).
- Keep water nearby. Ask for a minute to think — it's allowed and expected.
- If you blank: **go back to Section 4, step 1.** Smaller example. Always works.

---

## 8. The meta-trick

> Every hard problem is an easy problem wearing a costume.
> Your job is to **name the pattern under the costume**, not to be a genius.

Genius is not the bar. **Reliable pattern recognition + clean code + clear
communication** is the bar. This repo trains exactly those three. Go.
