# Jenkins pipeline example in OpenShift v3.5

Create a project called "cicd".

```oc new-project cicd```

Create an app from the console (or the cli) and name it <code>dev</code>. This will create the <em>development</em> build and deployment configurations.

```oc new-app --name=dev https://github.com/openshift/cakephp-ex.git```

Wait for the build to complete and manually tag the image stream.

```oc tag cicd/dev:latest cicd/dev:prod```

Create the <code>prod</code> deployment and expose it.

```oc new-app --image-stream=cicd/dev:prod --name=prod --allow-missing-images=true```

```oc expose svc prod```

Create the pipeline build config.

```oc new-build --strategy=pipeline https://github.com/bkoz/pipeline.git```

Now watch the pipeline progress from the OpenShift console <em>Builds</em> menu.

