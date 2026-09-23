### Exercise 4:
Postgres stores the metadata of the runs,like parameters, metrics, tags, run IDs, statuses. Minio stores artifacts, for example trained model files. 

Model files not stored in Postgres, because they can be large binary files.

### Exercise 5:

Now I can compare previous runs easily, including their parameters, metrics, artifacts. I can check if two runs with the same code and seed produce identical results.

I can reproduce a run using the recorded settings, and share the expirement history with others through the tracking server.
