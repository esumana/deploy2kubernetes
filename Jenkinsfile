pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh 'echo Docker Build'
        //sh "docker build -t kmlaydin/podinfo:${env.BUILD_NUMBER} ."
      }
    }
    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh 'echo Docker Push'
          //sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          //sh "docker push kmlaydin/podinfo:${env.BUILD_NUMBER}"
        }
      }
    }
    stage('Docker Remove Image') {
      steps {
        sh 'Docker Remove Image'
        //sh "docker rmi kmlaydin/podinfo:${env.BUILD_NUMBER}"
      }
    }
    stage('Apply Kubernetes Files') {
      steps {
          withKubeConfig([credentialsId: 'JENKINS-CONNECT-TO-REMOTE-K8S']) {
          sh 'echo Apply Kubernetes Files'
          //sh 'cat deployment.yaml | sed "s/{{BUILD_NUMBER}}/$BUILD_NUMBER/g" | kubectl apply -f -'
          //sh 'kubectl apply -f service.yaml'
        }
      }
   }
   stage('Deploy to Kubernetes') {
     steps {
       withCredentials([file(credentialsId: 'JENKINS-CONNECT-TO-REMOTE-K8S', variable: 'KUBECONFIG')]) {
         echo 'Deploy to Kubernetes'
         // Use the KUBECONFIG variable to interact with Kubernetes
         //sh 'kubectl --kubeconfig=$KUBECONFIG apply -f your-deployment.yaml'
       }
     }
    }

post {
    success {
      slackSend(message: "Pipeline is successfully completed.")
    }
    failure {
      slackSend(message: "Pipeline failed. Please check the logs.")
    }
}
  }
}
