# MLOps on Vertex AI Pipelines

When you run the cells and reach compile step, you are creating a pipeline.yaml file.
This YAML file is essentially the set of instructions that allows Vertex AI Pipeline Service to execute the ML workflow.

That means you're not setting up a VM in the background, and you don’t need to worry about security, logging, or orchestration. Instead, you're simply telling Vertex AI:

“Hey Vertex AI, execute this pipeline workflow,”

and it takes care of everything for me.

We specify individual components, then define a pipeline that wires the components together.

For example, in an end-to-end (E2E) ML workflow, depending on the use case, you might not want to deploy your model automatically.
You might only want to create the model artifact, which can then go through additional regression processes, move across different environments, or be tested with unit tests, integration tests, or A/B testing.

All of these can happen at different stages of the process.

For the purpose of this demo, the goal is to show how to create an E2E ML workflow by training a model and then deploying it as a unified pipeline.


# 1) Polished description (what this setup is doing)
## CI / CD / CT in plain terms

Continuous Integration (CI): teammates commit to the same repo; automated checks make sure changes build, tests pass, and nothing breaks.

Continuous Delivery (CDel): once code is “good,” you package it (e.g., a container image) and make it ready to ship.

Continuous Deployment (CDep): you ship and run that image in real environments (dev → test → prod).

Continuous Training (CT): for ML, you also need to retrain models when new data arrives or quality drops (e.g., p95 error ↑, accuracy ↓). CT is triggered by a scheduler (cron/Cloud Scheduler) or an event (e.g., message on Pub/Sub/Kafka) that says “retrain now.”

## What your specific flow does (high level)

Build an image that generates your pipeline
You’re not directly running an ML workflow inside this notebook. You create a container image that, when run, executes a small CLI/py script (pipeline.py) to produce or submit your pipeline.yaml (Kubeflow/Vertex spec).

## Store artifacts in GCP

Artifact Registry holds the image that knows how to build/submit your pipeline spec.

A GCS bucket stores the compiled pipeline.yaml (and other artifacts if you want).

Use Cloud Build to build/push the image

You can build locally (Docker) or use Cloud Build (managed CI/CD) with cloudbuild.yaml.

Cloud Build can be connected to your GitHub repo so every push/PR automatically rebuilds the image.

Scheduling / triggering retraining

A Cloud Scheduler job (cron-like) or an event (e.g., Kafka/Pub/Sub message) triggers a small job/notebook/script (think scheduler.ipynb/job) that runs the container.

That container creates or submits pipeline.yaml and kicks off your ML pipeline (e.g., weekly retrain, or retrain when model quality dips).

In short: Git push → Cloud Build builds image → Image is pushed to Artifact Registry → A scheduler/event runs the image → The image generates/submits pipeline.yaml → Vertex/Kubeflow runs the pipeline → (optionally) conditional deploy if metrics ≥ threshold.
