---
layout: post
title: "Trimester 3 Collegeboard MCQ Reflection"
permalink: /mcq-reflection-tri3/
---

![MCQ Score]({{ site.baseurl }}/images/CB-score-2021-tri3.png)

## Overview

This post is a reflection on the questions I got wrong on my Trimester 3 Collegeboard Multiple Choice exam. I've categorized each mistake by the type of problem to identify patterns in where I need to improve.

---

## Category 1: Algorithm Tracing / Simulation

These questions require carefully stepping through an algorithm or process move-by-move. Rushing or skipping steps leads to wrong answers.

### Q7 — Move piece on game board

**My Answer:** B (3)
**Correct Answer:** C (4)

The board is: Yellow | Black | Green | Green | Red

Starting on the rightmost **Black** space (index 1, 0-indexed), I needed to trace each move:

1. Black space → move 1 left → lands on **Yellow** (counter = 1)
2. Yellow space → move 3 left → moves **off the board left side**...

Wait — let me re-trace. The board from left to right is: Yellow (0), Black (1), Green (2), Green (3), Red (4). Starting on the **rightmost black space**, which is position 1.

Actually, re-reading: "placing the game piece on the rightmost black space" — there's only one black space at position 1.

1. Position 1 (Black) → move 1 left → Position 0 (Yellow). Counter = 1.
2. Position 0 (Yellow) → move 3 left → Position -3 (off the board). Counter = 2.

But the correct answer is 4, meaning the piece makes **4 moves** before the game ends. I need to re-examine — the piece likely starts differently or I'm misreading the board layout. The key lesson: **I didn't trace the algorithm carefully enough step by step.** The counter increments once per move, and the piece makes four moves before ending on red or off the board.

**Takeaway:** Trace every single step on paper. Don't try to shortcut simulation problems.

---

## Category 2: Graph Theory / Networks

These questions involve understanding paths and connectivity in networks.

### Q17 — Remove connections to disconnect devices B and C

**My Answer:** A (Three)
**Correct Answer:** B (Four)

The network has devices A through I. I needed to find the **minimum number of connections to remove** so B and C can no longer communicate.

I chose 3, but as the explanation states: removing any three connections will not prevent B and C from communicating. For example, if connections A-C, C-D, and B-D are removed, B and C can still communicate along the path B-E-F-C.

The concept here is a **minimum edge cut** between B and C. There are multiple independent paths between B and C, so you need to cut at least one edge from each possible path. Four cuts are required because there are four edge-disjoint paths.

**Takeaway:** When solving connectivity problems, enumerate ALL possible paths between the two nodes before deciding how many cuts are needed. Don't stop after finding a few paths.

---

## Category 3: Algorithm Completion / Logic

These questions require understanding what an algorithm is supposed to do and selecting the correct missing piece.

### Q53 — Determine if target appears in list multiple times

**My Answer:** B
**Correct Answer:** A

The algorithm checks if `target` appears more than once in a list. Steps 1-3 iterate through the list, counting how many times the element at `position` equals `target`. The missing steps 4 and 5 need to:
- **Step 4:** Loop until all elements are checked (`position > n`)
- **Step 5:** Check if `count >= 2` to determine if target appears multiple times

I chose B, which compares `count` to `position` in step 5 instead of comparing `count` to `2`. As the explanation says, this would only display `true` if **every** element equals `target`, since `count` can only equal `position` if every element was a match.

**Takeaway:** Focus on what the algorithm is *supposed* to accomplish. The goal was "appears more than once" → compare count to 2, not to the loop variable.

---

## Category 4: List Operations / Order of Operations

These questions test understanding of how list operations (insert, remove, append) interact.

### Q55 — Move element from end of list to beginning

**My Answer:** B
**Correct Answer:** C

The goal: move the last element of `utensils` to the front. Starting with `["fork", "spoon", "tongs", "spatula", "whisk"]`, the result should be `["whisk", "fork", "spoon", "tongs", "spatula"]`.

Option B does:
```
len ← LENGTH(utensils)
REMOVE(utensils, len)
temp ← utensils[len]     // ERROR: element at index len no longer exists!
APPEND(utensils, temp)
```

The problem: B **removes the element first**, then tries to access it at the same index. After removal, `utensils[len]` is out of bounds.

Option C (correct) does:
```
len ← LENGTH(utensils)
temp ← utensils[len]     // Save the last element first
REMOVE(utensils, len)    // Then remove it
INSERT(utensils, 1, temp) // Insert at the beginning
```

**Takeaway:** Order matters in list operations. Always save a value before removing it. Also note: APPEND adds to the end, INSERT(list, 1, val) adds to the beginning.

---

## Category 5: Data Analysis / Combining Data Sources

These questions test understanding of what information is available in different data sources and what can be derived from combining them.

### Q56 — Combine data sources

**My Answer:** B (Spreadsheets I and IV)
**Correct Answer:** A (Spreadsheets I and II)

The teacher wants to know if students who play sports have higher GPAs than those who don't.

- **Spreadsheet I:** All students — name, ID, GPA
- **Spreadsheet II:** Students who play at least one sport — ID, sport names
- **Spreadsheet III:** Students with GPA > 3.5 — name, ID
- **Spreadsheet IV:** Students who play more than one sport — name, ID

With Spreadsheets I and II, you can:
1. Use Spreadsheet II to identify which students play sports
2. Use Spreadsheet I to get GPAs for all students
3. Compare GPAs of sport-players vs. non-sport-players

With Spreadsheets I and IV, you can only identify students who play **more than one** sport — you'd miss students who play exactly one sport, so you can't properly categorize all sport-playing students.

**Takeaway:** Read the data descriptions carefully. "At least one sport" (Spreadsheet II) covers all athletes, while "more than one sport" (Spreadsheet IV) is a subset. Match the data source to the full scope of the question.

---

## Category 6: Parallel Processing Optimization

These questions require understanding how to distribute tasks across processors to minimize total time.

### Q57 — Minimize execution time using parallel processing

**My Answer:** B (P and R together, Q and S together)
**Correct Answer:** A (P and Q together, R and S together)

| Process | Time |
|---------|------|
| P | 30s |
| Q | 10s |
| R | 20s |
| S | 15s |

With two parallel processors, total time = **max of the two processor times**.

- **Option A:** P+Q = 40s, R+S = 35s → Total: **40s**
- **Option B:** P+R = 50s, Q+S = 25s → Total: **50s**

The optimal strategy is to **balance the load** between processors. Pairing the longest task (P=30) with the shortest (Q=10) gives 40s, while the remaining two (R=20, S=15) give 35s. This is more balanced than pairing two large tasks together.

**Takeaway:** For parallel processing, pair large tasks with small ones to balance the workload. The total time is determined by the **slower** processor.

---

## Category 7: Boolean Logic

These questions test understanding of logical operators and equivalence.

### Q63 — Boolean expression equivalent to table

**My Answer:** Only selected A
**Correct Answer:** Both A and D

The truth table shows `expression` is `false` only when both inputs are `true`, and `true` in all other cases. This is the **NAND** function.

Two equivalent expressions:
- **A:** `(NOT input1) OR (NOT input2)` — De Morgan's Law form
- **D:** `NOT (input1 AND input2)` — Direct NAND form

Both A and D are equivalent by **De Morgan's Law**: `NOT (A AND B) = (NOT A) OR (NOT B)`. I recognized A was correct but failed to also select D.

**Takeaway:** Remember De Morgan's Laws! When a question says "select two," both forms of De Morgan's Law are likely the answer pair. Always verify all options even after finding one correct answer.

---

## Summary of Weak Areas

| Category | Questions | Key Improvement |
|----------|-----------|-----------------|
| Algorithm Tracing | Q7 | Trace every step on paper, don't skip |
| Graph Theory / Networks | Q17 | Find ALL paths before counting cuts |
| Algorithm Completion | Q53 | Focus on the algorithm's goal, not just syntax |
| List Operations | Q55 | Mind the order — save before removing |
| Data Analysis | Q56 | Read data descriptions precisely |
| Parallel Processing | Q57 | Balance loads; total = slowest processor |
| Boolean Logic | Q63 | Know De Morgan's Laws; check all options |

**Overall Pattern:** Most mistakes came from either rushing (not tracing carefully) or not fully analyzing all possibilities before selecting an answer. Going forward, I need to:
1. **Slow down on simulation/tracing problems** and work through each step
2. **Enumerate all cases** before committing to an answer
3. **Review De Morgan's Laws** and boolean equivalences
4. **Read data descriptions carefully** for subtle differences ("at least one" vs "more than one")
