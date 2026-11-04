{
    "status": "ok",
    "model_version": "2",
    "run_id": "382d6dbed962411db5c4bd3a344ce61b",
    "gate_verdict": "GO",
    "schema_version": "1.0.0",
    "model_source": "registry champion"
}

## Exercise 1

1. Which requests did the server score, and which did it refuse? For each refusal, read the stack trace and find which component detected the problem.

    It scored for glucose = 0, glucose = -50, and age = 250 with BMI = 336. It refused for extra column, missing BMI, reordered columns. Every refusal was for scikit-learn's feature-name validation (_check_feature_names)

2. Which of the scored requests should never reach the model? Name the rule from the Week 5 contract that would stop each one.

    Glucose = 0, shouldnt reach because, it should be greater than 0.
    Glucose = -50, for the same reason.
    Age =250 and BMI = 336 shouldnt reach because, they are out of their ranges.

3. Name two more things your team would need from a prediction service that this server does not give you.

    It would need input validation, and an health endpoint, which reports the team if the service is ready, and which exact model version its running. 

## Exercise 3

1. Online took far longer for the same rows. Where does the time go? Name the steps that a request passes through and the batch call does not.

    The online requests must go through http request, json parsing, data-contract validation, DataFrame creation, response serialization. With batch I can avoid repeating these overheads for every patient.

2. Give one workload where batch scoring is the right choice, and one where online scoring is. What distinguishes them?

    Online scoring is for predicting one speecific patient's risk, when we need it instantly, while batch is great when we want thousands of patients' at a scheduled time.

3. Why does the table report p50 and p95, and not the mean?

    With p95 I can see how slow the slower requests are, with p50 I can see how long a typical requests take. With mean we wouldnt see how slow are the slower ones.

## Exercise 4

1. The registry is down when Kubernetes starts a new pod, for example to replace one that crashed. What happens with this image, and with an image that downloads the model from the registry when the container starts?

    The image could start as normal, because the model is included with the image, so the model doesnt need the MLFlow registry. The image which downloads at startup will fail to start until the registry is available again.