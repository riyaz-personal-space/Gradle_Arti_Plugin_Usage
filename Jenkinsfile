pipeline {
    agent any

    environment {
        
        ARTIFACTORY=credentials('ci-publish')
        
    }

    stages {
        stage('Hello') {
            steps {
                sh 'echo $ARTIFACTORY_USR'
                sh 'echo $ARTIFACTORY_PSW'
            }
        }
    
        stage('rtconfig') {
            steps{
                rtGradleResolver (id: "RESOLVER",serverId: "artifactory",repo: "ci-maven-virtual")
                rtGradleDeployer (id: "DEPLOYER",serverId: "artifactory",repo: "libs-snapshot-local")
            }
        }

        stage('deploy') {
            steps{
                rtBuildInfo (captureEnv: true)
                rtGradleRun (usesPlugin: "true",tool: "gradle",buildFile: 'build.gradle',tasks: 'clean artifactoryPublish',deployerId: "DEPLOYER",resolverId: "RESOLVER")
                rtPublishBuildInfo (serverId: "artifactory")

            }
        }

    }
}
