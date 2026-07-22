pipeline {
agent any

```
stages {

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t sgedam123/jenkins-devops:v1 .'
        }
    }

    stage('Docker Hub Login & Push') {
        steps {
            withCredentials([usernamePassword(
                credentialsId: 'cc02b3c9-66b9-4dce-99c0-a0d49296946f',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS'
            )]) {

                sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                docker push sgedam123/jenkins-devops:v1
                '''
            }
        }
    }

    stage('Deploy to Kubernetes') {
        steps {
            sh 'kubectl apply -f deployment.yml'
        }
    }
}
```

}

