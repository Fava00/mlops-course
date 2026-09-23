Exercise 4:
1. No, they didn't pick the same one. 
2. 88ecec6892f443d88cf803fa0219a379,
I selected rf-n_estimators=300 for registration because it had the highest F1 score in the sweep (0.6240) and also achieved the highest accuracy and recall among the six configurations. Although logistic regression with C=1.0 achieved a higher ROC-AUC, the random forest performed better at the current classification threshold. I would therefore register RF-300 as the current candidate model, while treating registration as traceability/versioning rather than proof that the model is ready for production. From the confusion matrices I can tell that this model has the least amount of false negatives which is very important in diabetes cases. Also it had the best true positive rate, with 39.
3. MlFlow recorded model parameters, metrics,tags, git commit for every run. So I didnt have to remember which configuration, code version produced a specific result.

Exercise 6
1. Traceability chain:
    1. With get_model_version_by_alias() it resolves the staging, getting the registered model version
    2. Read the version's run_id, identifying the source MlFlow run
    3. With get_run(run_id) retrieves the run's parameters, metrics, tags
    4. Read the git_commit tag and use Git for git show, git checkout, to inspect the commit representing the training code

2. Hop 4 gave me the recorded Git commit, so I could inspect the repository in that state. But the model had been trained with uncommitted changes, so that commit did not represent the state which the model was trained on.
Recording git_dirty tells me about this, but not tells me what were the uncommitted changes
3. No, because staging is still an evaluation environment, and a dirty run still could be useful.The cost is allowing dirty runs into staging is weaker reproducibility, traceabilty. But I would require a clean run before deploying to production, because we need the exact code for reproduction of a run. The benefit is experimentation is not blocked.
4. Aliases are more flexible, user can define them, rather then limiting to fixed sets. Its useful for example different international deployment, or a model version can be simultaneoulsy staging and champion aliases.
5. It only records the location of the dataset, but not the version of it. If somebody changes the file, another person may not be able to reproduce the same run. To close this gap, the dataset itself needs versioning.

Exercise 7
1. The champion and staging aliases were moved back to version 2. Registry confirms this.
The model versions didnt change, version 2 and 3 remained immutable, with artifacts, metrics, tags, parameters. The server would resolve models:/diabetes-classifier@champion to version 2.
2. It's tagged by rolled_back_at, rolled_back_to, rollback_reason, with these and earlier promotion tags, preserves the evidence. Without these an auditor wouldn't know that version 3 had been a champion before, or if it was replaced, and why it got rollbacked.
3. It was acceptable according to the rollback rule, version 2 had promoted_at tag, so that means it had previously passed the promotion process. But it shows, that tree state not recorded, so its recorded commit is not sure to be clean when the model was trained. So a better rollback target would be a previously promoted version which has git_dirty=false, so it's reproducable.