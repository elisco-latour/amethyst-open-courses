---
id: q_pyfun_03
assessment_id: python_fundamentals_v1
title: "Trace a for loop sum"
question_type: trace
sub_skill_tags: [python_fundamentals]
xp: 15
difficulty: intermediate
sort_order: 3
language: python
correct_index: 1
---

# code
total = 0
for i in range(1, 5):
    if i % 2 == 0:
        total += i
print(total)

<!-- SEPARATOR -->

# question
What does this snippet print?

<!-- SEPARATOR -->

# options
- 4
- 6
- 10
- 0

<!-- SEPARATOR -->

# explanation
`range(1, 5)` yields 1, 2, 3, 4. Only even values (2 and 4) are added: 2 + 4 = 6.
