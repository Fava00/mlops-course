## Exercise 2:

1. Which group is the model worst on, and by how much against the aggregate? How far can you trust that gap, given the group's n?

The model is the worst on the 50+age group, with a ROC AUC of 0.6071 compared to the aggregate ROC AUC of 0.8345. The false-positive rate is also higher (0.4167) than the 21-29 group. However this group only contains 19 patients, including 7 positive cases, so this may not generalize to larger population.

2. What can you do with this finding now, and what would you need before you act on it?

I can flag this group for further investigation, analyze false positives, false negatives, and monitor the performance. Before changing the model, i would need a larger and more representative test sample. Depending on that test samples findings, I could consider recalibrating the model.

3. One group has an F1 of 0.0000 and a ROC AUC of 1.0000 on the same patients. How can both be correct, and what do they tell you about that group?

The under_normal BMI group has 30 patients, and only 1 positive case. Model predicts every patient as negative, resulting in zero recall and F1.

## Exercise 4

1. Week 5's gate worked because prepare depended on the verdict file. Why can that not work for this gate, and what replaces it?

Week 5 gate checked the input data before preprocessing and training, the prepare stage depended on its verdict.

Week 6 gate evaluates the trained model, so it runs after evaluation stage. There is no stage to depend on its verdict, because this is the last stage in DVC pipeline.
Instead it stops dvc repro on NO-GO, register and rpomote explicitly checks require_go()

2. What happens if register does not call require_go? Who would notice, and when?

Without require_go(), a model could be registered, even though failing the gate. Problem might only be discovered during review by developers, deployment by deployment team when releasing model.

3. With C: 0.005, F1 fell by 0.1098 and ROC AUC rose. Which one is the right answer? What do you need to know about how the model is used, and what does acceptance.regression.primary_metric commit you to?

Both answer correct. I need to know how the model is used, and what are the consequences of false positive, false negatives. For diabetes prevalence, missing an actual positive is more harmful.

By setting acceptance.regression.primary_metric, it commits me to rejecting models, which F1 drops more than allowed amount comparet to the champion, even if ROC AUC imrpoves.

## Exercise 5

1. age_band has a selection-rate gap over 0.30, yet you gate only its TPR gap. Why is the large selection-rate gap defensible?

Diabetes prevalence differs largely between ge groups, so different prediction rates can occur. We gate TPR, becuase consistent identifying of actual positive cases are more important.

2. The TPR limit on age_band is looser than on insulin_missing. Why, and what would make you tighten it?

Because age groups have larger TPR gap (0,2059 vs 0,1562), also some groups have smaller sample sizes. It would make me tighten it if there would be more representative data.

3. This dataset has no sex, ethnicity, income or clinic column. Name one group you would want to audit this model for and cannot. What would have had to happen, and when?

It would be great to compare different clinics. Clinic information would have needed to be collected, and enough patient data needed to be collected from each clinic.


