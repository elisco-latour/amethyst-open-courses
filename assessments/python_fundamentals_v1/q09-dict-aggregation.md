---
id: q_pyfun_09
assessment_id: python_fundamentals_v1
title: "Dict aggregation"
question_type: trace
sub_skill_tags: [data_engineering]
xp: 20
difficulty: advanced
sort_order: 9
language: python
correct_index: 0
---

# code
rows = [
    {"region": "EU", "amount": 10},
    {"region": "US", "amount": 5},
    {"region": "EU", "amount": 7},
]
totals = {}
for r in rows:
    totals[r["region"]] = totals.get(r["region"], 0) + r["amount"]
print(totals["EU"])

<!-- SEPARATOR -->

# question
What does this snippet print?

<!-- SEPARATOR -->

# options
- 17
- 10
- 7
- KeyError

<!-- SEPARATOR -->

# explanation
EU appears in the first and third rows. `dict.get("EU", 0)` defaults to 0 the first time, then sums to 10 + 7 = 17.
