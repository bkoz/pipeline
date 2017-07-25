# Jenkins pipeline example in OpenShift v3.5

Create a project called <em>cicd</em>.

```oc new-project cicd```

Create an app and name it <em>dev</em>. This will create the <em>development</em> build and deployment configurations.

```oc new-app --name=dev https://github.com/openshift/cakephp-ex.git```

```oc expose service dev```

Wait for the build to complete and manually tag the image stream.

```oc tag cicd/dev:latest cicd/dev:prod```

Create the <em>prod</em> deployment and expose it.

```oc new-app --image-stream=cicd/dev:prod --name=prod --allow-missing-images=true```

```oc expose svc prod```

Create the pipeline build config.

```oc new-build --strategy=pipeline https://github.com/bkoz/pipeline.git```

From the OpenShift console, navigate to <em>Builds->Pipelines</em> and watch the progress.

This file is published at [https://bkoz.github.io/pipeline/](https://bkoz.github.io/pipeline/)
