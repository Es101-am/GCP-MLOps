*** MLOps on Vertex AI Pipelines
When you run the cells and reach compile step, you are creating a pipeline.yaml file.
This YAML file is essentially the set of instructions that allows Vertex AI Pipeline Service to execute the ML workflow.

That means I’m not setting up a VM in the background, and I don’t need to worry about security, logging, or orchestration. Instead, I’m simply telling Vertex AI:

“Hey Vertex AI, execute this pipeline workflow,”

and it takes care of everything for me.

We specify individual components, then define a pipeline that wires the components together.

For example, in an end-to-end (E2E) ML workflow, depending on the use case, you might not want to deploy your model automatically.
You might only want to create the model artifact, which can then go through additional regression processes, move across different environments, or be tested with unit tests, integration tests, or A/B testing.

All of these can happen at different stages of the process.

For the purpose of this demo, the goal is to show how to create an E2E ML workflow by training a model and then deploying it as a unified pipeline.
