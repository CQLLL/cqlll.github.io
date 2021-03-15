---
layout: post
title: Kaggle Competition Riiid
---

Tailoring education to a student's ability level is one of the many valuable things an AI tutor can do. Your challenge in this competition is a version of that overall task; you will predict whether students are able to answer their next questions correctly. You'll be provided with the same sorts of information a complete education app would have: that student's historic performance, the performance of other students on the same question, metadata about the question itself, and more.


## Files
### train.csv
<li>
<code>row_id</code>: (int64) ID code for the row
</li>

<li>
<code>timestamp</code>: (int64) the time in milliseconds between this user interaction and the first event completion from that user.
</li>

<li>
<code>user_id</code>: (int32) ID code for the user.
</li>

<li>
<code>content_id</code>: (int16) ID code for the user interaction
</li>

<li>
<code>content_type_id</code>: (int8) 0 if the event was a question being posed to the user, 1 if the event was the user watching a lecture.
</li>

<li>
<code>task_container_id</code>: (int16) Id code for the batch of questions or lectures. For example, a user might see three questions in a row before seeing the explanations for any of them. Those three would all share a task_container_id.
</li>

<li>
<code>user_answer</code>: (int8) the user's answer to the question, if any. Read -1 as null, for lectures.(lectures don't need be answered)
</li>


<li>
<code>answered_correctly</code>: (int8) if the user responded correctly. Read -1 as null, for lectures.
</li>

<li>
<code>prior_question_elapsed_time</code>: (float32) The average time in milliseconds it took a user to answer each question in the previous question bundle, ignoring any lectures in between. Is null for a user's first question bundle or lecture. Note that the time is the average time a user took to solve each question in the previous bundle.
</li>


<li>
<code>prior_question_had_explanation</code>: (bool) Whether or not the user saw an explanation and the correct response(s) after answering the previous question bundle, ignoring any lectures in between. The value is shared across a single question bundle, and is null for a user's first question bundle or lecture. 

Typically the first several questions a user sees were part of an onboarding diagnostic test where they did not get any feedback.
</li>

### question.csv

<li>
<code>question_id</code>: foreign key for the train/test content_id column, when the content type is question (0).
</li>


<li>
<code>bundle_id</code>: code for which questions are served together.
</li>

<li>
<code>correct_answer</code>: the answer to the question. Can be compared with the train user_answer column to check if the user was right.
</li>

<li>
<code>part</code>: the relevant section of the TOEIC test.
</li>

<li>
<code>tags</code>: one or more detailed tag codes for the question. The meaning of the tags will not be provided, but these codes are sufficient for clustering the questions together.
</li>


### lectures.csv
metadata for the lectures watched by users as they progress in their education.

<li>
<code>lecture_id</code>: foreign key for the train/test content_id column, when the content type is lecture (1).
</li>

<li>
<code>part</code>: top level category code for the lecture.
</li>

<li>
<code>tag</code>: one tag codes for the lecture. The meaning of the tags will not be provided, but these codes are sufficient for clustering the lectures together.
</li>
<li>
<code>type_of</code>: brief description of the core purpose of the lecture
</li>


### example_test_rows.csv 
Three sample groups of the test set data as it will be delivered by the time-series API. The format is largely the same as train.csv. There are two different columns that mirror what information the AI tutor actually has available at any given time, but with the user interactions grouped together for the sake of API performance rather than strictly showing information for a single user at a time. Some users will appear in the hidden test set that have NOT been presented in the train set, emulating the challenge of quickly adapting to modeling new arrivals to a website.

<li>
<code>prior_group_responses</code>
(string) provides all of the user_answer entries for previous group in a string representation of a list in the first row of the group. All other rows in each group are null. If you are using Python, you will likely want to call eval on the non-null rows. Some rows may be null, or empty lists.
</li>

<li>
<code>prior_group_answers_correct</code>
(string) provides all the answered_correctly field for previous group, with the same format and caveats as prior_group_responses. Some rows may be null, or empty lists.
</li>


## Seaborn draw picture

```python

import seaborn as sns 
plt.figure(figsize=(16,10),dpi = 80)
sns.kdeplot(df['name'],shade = True,color="g",alpha=.7)
plt.legend()
plt.show()

```

