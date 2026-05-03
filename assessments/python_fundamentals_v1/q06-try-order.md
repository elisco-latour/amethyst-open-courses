---
id: q_pyfun_06
assessment_id: python_fundamentals_v1
title: "Exception handling order"
question_type: ordering
sub_skill_tags: [software_quality]
xp: 15
difficulty: intermediate
sort_order: 6
correct_order: [1, 0, 3, 2]
---

# prompt
Order the keywords as they appear in a complete `try` statement that runs cleanup whether or not an error occurred:

<!-- SEPARATOR -->

# items
- except
- try
- finally
- else

<!-- SEPARATOR -->

# explanation
The canonical order is `try → except → else → finally`. `else` runs when no exception was raised; `finally` always runs.
