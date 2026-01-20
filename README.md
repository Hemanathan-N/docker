This Repo shows Examples of Dockerfile && Jenkinsfile and also have basic Calculator-Application


docker run -d --name jenkins --network=host -p 8080:8080 -p 50000:50000 -v ~/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v ~/.kube:/var/jenkins_home/.kube -v ~/.minikube:/home/hema/.minikube  --group-add 108 hemanathan18/jenkins-devops:latest
