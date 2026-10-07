## Exercise 2:

1. I would add the notes column to the ingestion contract, because the clinic said it is an intentional field
and will be present in every batch from now on. I wouldnt drop it before validation, because the inegstion
contract wouldn't accurately descibe the data supplied. Th clinic should agree with the team that's responsible
for the data contract, ML pipeline.
	
2. The four faults are:

	- No specific row: unexpected notes column present.
	- Row 3, glucose had value "unknown"
	- Row 7, age had value 250, which is higher then the maximum
	- Row 23, bmi had value 280.0, which is higher then the maximum

	The Row 3 fault causes 4 report lines, Pandera can not turn it into float64, it doesn't satisfy the float 64 dtype, it is not satisfy the lower, and upperbound checks because of this neither.

## Exercise 4:

1. Without the gate the one with age = 250 reached data preparation stage. It reached data/processed/train.csv, which could be used by training stage to produce the model. After adding the gate, validation stops the pipeline before prepare, so the training data remains unchanged when an invalid batch is supplied.
2. Because the validation failed before, the prepare will refuse to continue.
It produces that because of this part of the code:

	if not _require_validated(settings).get("passed"):
        raise ValidationFailed(
            "The last validation failed. Fix the data before you prepare it."
        )

	The edge in dvc.yaml is not enough on it's own, because it only controls execution when DVC is running the pipeline. The user could invoke the python command directly, bypassing the DVC.
		
3. Changing te MAX_AGE, changes the validation contract, so validate stage needs to run again. The dataset still passes and actual training data does not change, so prepare, train, and evaluate can be skipped. Same happens when changing it back to 120.

	Model does not need to be trained again, because only the validation rule changed, not the training data.

## Exercise 6:

1. As long as null means that the test was not done, I would allow filling the model with the missing insulin value with the training median.

	The clinical expert and the model team should decide whether this is acceptable.
