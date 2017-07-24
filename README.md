# Jenkins pipeline in OpenShift

Create a project called "cicd".

oc new-project cicd

Create a php app from the console (or the cli) and name it "cake".

Create the pipeline build config.

oc new-build --strategy=pipeline https://github.com/bkoz/pipeline.git

oc tag cicd/cake:latest cicd/cake:prod

oc new-app --image-stream=cicd/cake:prod --name=prod --allow-missing-images=true

oc expose svc prod



