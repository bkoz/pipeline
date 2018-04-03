# Jenkins pipeline example in 

Create a project called <em>cicd</em>.

```oc new-project cicd```

## OpenShift v3.7

```
oc new-app jenkins-ephemeral
oc new-app -f https://raw.githubusercontent.com/openshift/origin/master/examples/jenkins/application-template.json
```

Create the pipeline build configuration (via cli or import yaml from console).

Console: Add to project->import the following yaml

```
kind: "BuildConfig"
apiVersion: "v1"
metadata:
  name: "pipeline"
spec:
  strategy:
    jenkinsPipelineStrategy:
      jenkinsfile: |-
        node() {
          stage 'buildFrontEnd'
          openshiftBuild(buildConfig: 'frontend', showBuildLogs: 'true')
  
          stage 'deployFrontEnd'
          openshiftDeploy(deploymentConfig: 'frontend')
  
          stage "promoteToProd"
          input message: 'Promote to production ?', ok: '\'Yes\''
          openshiftTag(sourceStream: 'origin-nodejs-sample', sourceTag: 'latest', destinationStream: 'origin-nodejs-sample', destinationTag: 'prod')
  
          stage 'scaleUp'
          openshiftScale(deploymentConfig: 'frontend-prod',replicaCount: '2')
        }
```

CLI:

```
oc create -f - <<EOF
<Copy and paste the YAML code from above>
EOF
```

Builds->Pipelines

Click on the "pipeline" link.

Click on "Configuration".

Note the (3) stages.

Click on "Start Pipeline".

Click on "History".

Under the Build #, click on "View Log" -> Redirects to Jenkins Console

OCP console -> view frontend route

Builds->Pipeline

Input Required via Jenkins console.

Note delpyment with 2 replicas.

oc create route --service=frontend-prod edge (Also possible via console)

## OpenShift v3.5

Create an app and name it <em>dev</em>. This will create the <em>dev</em> build and deployment configurations.

```oc new-app --name=dev https://github.com/openshift/cakephp-ex.git```

```oc expose service dev```

Wait for the build to complete and manually tag the image stream so the <em>prod</em> deployment can be created.

```oc tag cicd/dev:latest cicd/dev:prod```

Create the <em>prod</em> deployment and expose it.

```oc new-app --image-stream=cicd/dev:prod --name=prod --allow-missing-images=true```

```oc expose svc prod```

Create the Jenkins pipeline build config using an example from my github repo.

```oc new-build --strategy=pipeline https://github.com/bkoz/pipeline.git```

From the OpenShift console, navigate to <em>Builds->Pipelines</em> and watch the progress.

This file is published at [https://bkoz.github.io/pipeline/](https://bkoz.github.io/pipeline/)
