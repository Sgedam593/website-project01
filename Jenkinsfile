pipeline {
agent any


stages {

    stage('Build Docker Image') {
        steps {
            sh "docker build -t sgedam123/jenkins-devops:${BUILD_NUMBER} ."
        }
    }

    stage('Docker Hub Login & Push') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: 'f63a91b6-ecec-466c-9da1-21a25494c30c',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {

                sh """
                echo \$DOCKER_PASS | docker login -u \$DOCKER_USER --password-stdin
                docker push sgedam123/jenkins-devops:${BUILD_NUMBER}
                """
            }
        }
    }

    stage('Deploy to Kubernetes') {
        steps {
            sh 'kubectl apply -f deployment.yml'
            sh "kubectl set image deployment/website \
jenkins-container=sgedam123/jenkins-devops:${BUILD_NUMBER}"
            sh "kubectl rollout status deployment/website"
        }
    }
  }
}
