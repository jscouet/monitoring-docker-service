pipeline {
    agent { node { label 'docker' } }


    environment {
        MYDOMAIN = credentials('my-service-domain')
    }
        
    stages {
        stage("Where") {
            steps {
                sh 'ls -la'
                sh 'pwd'
            }
        }

        stage("Deploy") {
            agent { node { label 'docker' } }
            steps {
                withDockerServer([credentialsId: '8cacf70b-c496-4719-a704-b90b8accc881', uri: 'tcp://service.jsoude.net:2376']) {
                    withDockerRegistry([credentialsId: '42cce0f9-7a02-4137-8273-5101f614328e', url: 'https://index.docker.io/v1/']) {
                        sh 'cd monitoring-docker-service'
                        sh 'docker-compose up -d'
                    }   
                }
            }
        }
                
        stage("clean workspace") {
            steps {
                cleanWs()
            }
        }
    }
    post {
        failure {
            slackSend baseUrl: 'https://couet.slack.com/services/hooks/jenkins-ci/', channel: 'code', color: 'bad', message: "pipeline failed - ${env.JOB_NAME} - ${env.BUILD_NUMBER} see ${env.BUILD_URL}", tokenCredentialId: 'JENKINS_SLACK_TOKEN'
        }
        success {
            slackSend baseUrl: 'https://couet.slack.com/services/hooks/jenkins-ci/', channel: 'code', color: 'good', message: "pipeline success - ${env.JOB_NAME} - ${env.BUILD_NUMBER} see ${env.BUILD_URL}", tokenCredentialId: 'JENKINS_SLACK_TOKEN'
        }
		always {
			echo "TAG_NAME   ${env.TAG_NAME}"
		}
    }
}
