node {
       stage 'buildInDev'
       openshiftBuild(buildConfig: 'dev', showBuildLogs: 'true')
       stage 'deployInDev'
       openshiftVerifyDeployment(deploymentConfig: 'dev')
       stage "deployToProd"
       input message: 'Promote to production ?', ok: '\'Yes\''
       echo "Deploying to production."
       openshiftTag(sourceStream: 'dev', sourceTag: 'latest', destinationStream: 'dev', destinationTag: 'prod')
       openshiftScale(deploymentConfig: 'prod',replicaCount: '2')
}
