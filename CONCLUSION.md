# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Layer 1 changed the results the most. The bare agent had 16 correct answers, 4 wrong answers and 4 unanswered questions. After adding layer 1, it had 18 correct answers, 6 wrong answers and no unanswered questions. It helped the agent work after tool errors, but some answers were still wrong. Layer 2 did not change the results. Layer 3 kept the same number of correct answers but changed 2 wrong answers into unverified answers. However, the confidence intervals for the bare agent [0.47, 0.82] and layer 1 [0.55, 0.88] overlap. So, we cannot say that the improvement is statistically proven with only 24 questions.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

The full harness did not fix q08. The agent used compare_regions and got a ranking from highest to lowest. But it answered "Jambyl" instead of choosing the last region, which had the lowest value. The grounding check did not catch this mistake because Jambyl was in the tool results. It checked if the answer was in the results, but not if the ranking was correct. I would add a simple code check to select the correct region from the ranking. This would add some code and time, but would not need another model call or more tokens.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would choose a workflow for monthly rankings because it gave better results on fixed questions: 13 out of 15 correct answers, compared with 9 out of 15 for the bare agent. It also used fewer model calls (15 vs. 26), fewer tokens (3852 vs. 19835) and less time (16.1 vs. 49.2 seconds). The full harness still made mistakes on q08 and q11, even with all three layers. The workflow could not answer q21 and q22 because of old region names, not because of ranking calculations. I would choose an agent for open-ended questions because a fixed workflow cannot answer every type of question.

---

I wrote this file myself, without any AI tool.
