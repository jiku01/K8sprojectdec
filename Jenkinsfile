pipeline {
    agent any

    stages {
        stage('Pull Code From GitHub') {
            steps {
                git url: 'https://github.com/jiku01/K8sprojectdec.git', branch: 'main'
            }
        }

        stage('Build the Docker image') {
            steps {
                sh 'docker build -t newimage /var/lib/jenkins/workspace/pipelinejob'
                sh 'docker tag newimage jiku01/newimage:latest'
                sh 'docker tag newimage jiku01/newimage:${BUILD_NUMBER}'
            }
        }
            stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push jiku01/newimage:latest'
                sh 'sudo docker image push jiku01/newimage:${BUILD_NUMBER}'
            }
        }
            stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /var/lib/jenkins/workspace/pipelinejob/pod.yml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
