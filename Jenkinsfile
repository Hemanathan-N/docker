pipeline {
    agent any
    environment {
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
}
stages {
    
    stage('checkout') {
        steps {
            git changelog: false, poll: false, url: 'https://github.com/Hemanathan-N/docker.git'
            }
        }

    stage('docker build') {
        steps {
            sh 'docker build -t hemanathan18/calculator:$docker_version .'
            }
    }
    
    stage('docker push') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'docker_pwd', usernameVariable: 'docker_un')]) {
                sh 'docker login -u ${docker_un} -p ${docker_pwd}'
              }
              sh 'docker push hemanathan18/calculator:$docker_version'
            }
        }
    
    stage('k8s deployment') {
        steps {
            sh '''
            minikube start --driver=docker --memory=2048 --cpus=2 || true
            minikube status
            kubectl get nodes
            kubectl get deploy
            echo "Workspace files:"
                ls -l
            kubectl apply -f deploy.yml
            '''
            }
         }
}

}







