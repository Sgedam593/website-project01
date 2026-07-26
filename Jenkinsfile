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
                credentialsId: '81d537ab-8671-4840-b9e8-ab9e214c2f13',
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
