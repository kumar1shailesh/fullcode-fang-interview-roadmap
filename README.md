# 🚀 FAANG Interview Roadmap (Python) — from zero to offer

A no-nonsense plan built around the **LeetCode 75** list. The goal is not to
grind 500 problems. It is to learn **~14 patterns** so well that any new problem
becomes "oh, that's just a sliding window with a hashmap."

> **The core insight:** FAANG interviewers reuse ~14 patterns. Interviewers are
> not testing whether you *memorised* a problem — they are testing whether you
> can **recognise which pattern** a disguised problem belongs to, then code it
> cleanly while talking. This roadmap trains recognition, not memorisation.

---

## How to use this repo

```
faang-interview-roadmap/
├── ROADMAP.md              ← you are here (the plan)
├── INTERVIEW_TRICKS.md     ← how to behave in the room + recognition table
├── README.md              ← quick start
├── cheatsheet.py           ← copy-paste templates for every pattern
├── patterns/               ← 14 files, each = 1 pattern + solved LC75 problems
│   ├── 01_two_pointers.py
│   ├── 02_sliding_window.py
│   ├── ...
│   └── 14_trie.py
└── tests/
    └── test_all.py         ← runs every solution to prove they're correct
```

Each `patterns/*.py` file follows the same shape:
1. **The pattern** in one paragraph of plain English.
2. **The trigger** — the words in a problem that scream "use me".
3. **The template** — reusable skeleton.
4. **2–4 real LeetCode 75 problems**, each explained line-by-line.
5. **Runnable tests** at the bottom (`python patterns/01_two_pointers.py`).

**Study loop for each file:** read the pattern → read the template → cover the
solution and try it yourself → check against the explained version → run the file.

---

## The 8-week plan

Each week = one focused theme. Do **the file, then re-solve 2 problems from
memory the next morning**. Spaced repetition is what makes patterns stick.

| Week | Theme | Files | Why this order |
|------|-------|-------|----------------|
| **1** | Arrays foundations | `01_two_pointers`, `02_sliding_window` | 40% of easy/medium array problems. Builds pointer intuition. |
| **2** | Lookups & running totals | `03_hashmap_set`, `04_prefix_sum` | O(1) lookups + O(n) range sums unlock a huge class of problems. |
| **3** | LIFO & sequences | `05_stack`, `06_linked_list` | Stack (incl. monotonic) + pointer surgery on lists. |
| **4** | Search | `07_binary_search` | "Binary search on the answer" is the #1 medium trick people miss. |
| **5** | Trees | `08_trees` | DFS + BFS. Recursion confidence. Half of all onsite rounds. |
| **6** | Graphs | `09_graphs` | BFS/DFS on grids & adjacency. Same recursion, new shape. |
| **7** | Heaps & backtracking | `10_heap`, `11_backtracking` | "Top-K" and "all combinations" problems. |
| **8** | DP + intervals + trie | `12_dynamic_programming`, `13_intervals`, `14_trie` | The hardest to *invent* — but very pattern-able once seen. |

**Weeks 9+ (maintenance):** 2 random problems/day, 1 mock interview/week. Rotate
through `cheatsheet.py` templates until you can write each from memory in <2 min.

---

## The difficulty ramp (don't skip)

```
   EASY  →  MEDIUM  →  "MEDIUM that looks HARD"
   (learn    (apply       (recognise the disguise —
    the       the          this is where offers are
    pattern)  pattern)     won or lost)
```

Most FAANG questions are **medium**. Hard questions are rare and usually a
medium + one twist. If you can reliably solve mediums while narrating your
thinking, you pass most loops.

---

## Complexity targets to memorise

| You see... | Aim for | Red flag if you're at... |
|------------|---------|--------------------------|
| Single array, "pair/triplet" | O(n) or O(n log n) | O(n²) brute force → look for two-pointer/hashmap |
| "Subarray/substring" | O(n) sliding window | nested loops |
| "Sorted array", "find X" | O(log n) binary search | O(n) scan |
| Tree/graph traversal | O(V+E) / O(n) | revisiting nodes |
| "All combinations/permutations" | O(2ⁿ) or O(n!) — unavoidable | but prune with backtracking |
| "Top K" | O(n log k) heap | full sort O(n log n) |

State your complexity **before** coding. Interviewers love it.

---

## The 4 sentences that pass interviews

Say these out loud in every problem:

1. **Clarify:** *"Can the input be empty? Are there duplicates? Is it sorted?"*
2. **Brute force first:** *"The naive way is O(n²) — nested loops. Let me improve it."*
3. **Name the pattern:** *"Since we need a pair summing to a target and it's sorted, this is a two-pointer problem."*
4. **Verify:** *"Let me trace it on `[2,7,11]` with target 9… returns [0,1]. Correct."*

A silent candidate who codes the perfect solution often loses to a talkative one
with a small bug. **Communication is scored.**

---

## What "advanced" actually means here

Beginners memorise solutions. Advanced candidates:
- **Derive** the solution from the pattern in real time.
- Know the **trade-offs** (time vs space, sort vs hashmap).
- Handle **edge cases before being asked** (empty, single element, all same, negatives).
- Can **optimise on request** ("can you do it in O(1) space?").

Every pattern file ends with an **"Interviewer follow-ups"** section training exactly this.

Start with [`patterns/01_two_pointers.py`](patterns/01_two_pointers.py). Then read [`INTERVIEW_TRICKS.md`](INTERVIEW_TRICKS.md).
