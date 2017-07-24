node {
       stage 'buildInDev'
       openshiftBuild(buildConfig: 'cake', showBuildLogs: 'true')
       stage 'deployInDev'
       openshiftVerifyDeployment(deploymentConfig: 'cake')
       stage "deployToProd"
       input message: 'Promote to production ?', ok: '\'Yes\''
       echo "Deploying to production."
       openshiftTag(sourceStream: 'origin-nodejs-sample', sourceTag: 'latest', destinationStream: 'origin-nodejs-sample', destinationTag: 'prod')
       openshiftScale(deploymentConfig: 'frontend-prod',replicaCount: '1')
}
