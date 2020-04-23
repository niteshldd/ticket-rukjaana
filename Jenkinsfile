
pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh "docker build -t niteshldd/rukjaana-ticket:latest ."
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh "docker push niteshldd/rukjaana-ticket:latest"
        }
      }
    }
    stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://api.kube.rukjaana.com']) {
          sh 'kubectl get all'
          sh 'kubectl rollout restart deployment/ticket-ruk-deployment -n development'
        }
      }
  }
}
}
