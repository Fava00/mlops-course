Exercise 1:
By changing the random seed, I changed which samples the model was trained and evaluated on. If the random seed changes it is difficult to reproduce and compare results.
Exercise 2:
I trained a RandomForestClassifier.
My best result was, when i used N_ESTIMATORS=4, MAX_DEPTH=7, PIPELINE_N_JOBS=2.
The result was for f1: 0.6446
Exercise 4: The erros is being raised by the config.py validation module, because PIPELINE_TEST_SIZE=1.5 is outside of the valid range for a test split (should be between 0 and 1)