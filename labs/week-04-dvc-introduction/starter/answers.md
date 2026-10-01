# Exercise 2:

### 1.
Git gained DVC pointer file, data/measurements.csv.dvc, and the raw batch, and gitignore change.
The DVC pointer is only 97 bytes.

Minio gained the actual contnets of data/measurements.csv, which was around 19000 bytes.

Idea behind DVS is that Git only needs to version a small pointer, which identifies the dataset, while the dataset is stored in remote storage, like Minio.
 

### 2.
Build_measurements make the output deterministic by reading the batch files in a fixed filename order
using batch_paths. And also by writing the CSV withput pandas index, with a fixed \n line terminator.


### 3.
They run uv run dvc pull data/measurements.csv. For it to work, the dataset version which was referenced by the dvc file must exist in the configured dvc remote.
And also he/she must have valid credentials for the Minio remote.

# Exercise 4:

### 1

git checkout <version 1> changed the DVC poiner stored in Git. It made the pointer refer to version 1 of the dataset
dvc checkout then changed the actual dataset file in the workspace.
Git can not restore the dataset, because they are not stored in Git.
DVC can not decide the version which should be used, unless it was selected through Git.


### 2

The Git checkout of the version 1 pointer works, because the .dvc pointer is stored in Git.
But dvc checkout fails if the version1 is not present in local DVC cache, because this only restores
data which is already available locally.


# Exercise 5:

### 1.
Dependencies of evaluate stage are:
      - models/model.pkl
      - models/mlflow_run_id.json
      - data/processed/test.csv
      - src/week_04_dvc_introduction/pipeline.py
      - src/week_04_dvc_introduction/model.py

If models/model.pkl is missing, then DVC would not know that evaluate depends on the trained model.

### 2.

The datasets not necessarily the same despite the same number of rows. The actual values, row ordering
can differ.

These can affect the trained model and its metrics. That's whx DVC uses content hash instead of
filename or row count.

# Exercise 6

### 1.

DVC MD5 is a hash of the actual bytes of data/measurements.csv, identifies exact version of the file
stored by dvc.

Mlflow's digest is MLflows own identifier of the Dataset object that was logged as an input to the run.

For an auditor i would give the DVC MD5, because thats directly identifies the versioned file bytes
stored in the DVC remote.

### 2.
The filter string used was: 
tags.dvc_md5 = 'a8fd7b4f0d6d1bc4e378a8f76c5fff0c'

It answers the question, which mlflow runs were trained using this exact version of the dataset?

### 3.
The new line is:

5. Data version: a8fd7b4f0d6d1bc4e378a8f76c5fff0c

The chain is now complete down to the exact versioned dataset bytes. In week 3 it stopped at the file path.

