node {
       stage 'buildInDev'
       openshiftBuild(buildConfig: 'cake', showBuildLogs: 'true')
       stage 'deployInDev'
       openshiftVerifyDeployment(deploymentConfig: 'cake')
       stage "deployToProd"
       input message: 'Promote to production ?', ok: '\'Yes\''
       echo "Deploying to production."
       openshiftTag(sourceStream: 'cake', sourceTag: 'latest', destinationStream: 'cake', destinationTag: 'prod')
       openshiftScale(deploymentConfig: 'prod',replicaCount: '2')
}
