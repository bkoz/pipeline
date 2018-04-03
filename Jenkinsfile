
       node() {
          stage 'buildFrontEnd'
          openshiftBuild(buildConfig: 'frontend', showBuildLogs: 'true')
  
          stage 'deployFrontEnd'
          openshiftDeploy(deploymentConfig: 'frontend')
  
          stage 'verifyFrontEnd'
          openshiftVerifyDeployment(deploymentConfig: 'frontend')
  
          stage "promoteToProd"
          input message: 'Promote to production ?', ok: '\'Yes\''
          openshiftTag(sourceStream: 'origin-nodejs-sample', sourceTag: 'latest', destinationStream: 'origin-nodejs-sample', destinationTag: 'prod')
  
          stage 'scaleUp'
          openshiftScale(deploymentConfig: 'frontend-prod',replicaCount: '2')
        }
