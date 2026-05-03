---
id: q_pyfun_07
assessment_id: python_fundamentals_v1
title: "Trace exception handling"
question_type: trace
sub_skill_tags: [software_quality]
xp: 20
difficulty: advanced
sort_order: 7
language: python
correct_index: 2
---

# code
def safe_div(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return "zero"
    finally:
        print("done")

print(safe_div(6, 0))

<!-- SEPARATOR -->

# question
What does this program output (in order)?

<!-- SEPARATOR -->

# options
- zero
- done
- done\nzero
- 0\ndone

<!-- SEPARATOR -->

# explanation
`finally` runs before the function returns, so `done` is printed first. Then `safe_div` returns `"zero"`, which the outer `print` displays.
