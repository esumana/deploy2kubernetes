//# SCM SHOULD CLONE YOUR GITHUB REPOSITORY BEFORE PIPELINE STARTS
pipeline {
  agent any
  //tools {
  //  jdk 'jdk 1.8'
  //}
  stages {
  //  # BUILD YOUR PROJECT
    stage('BuildAndUnitTest') {
      steps {
        sh 'echo BuildAndUnitTest!'
        //sh "sudo ./gradlew clean build"
      }
    }
    //# REMOVE OLD KUBECONFIG IF IT EXISTS
     stage('remove kubeconfig if exists') {
           steps {
             sh 'echo remove kubeconfig if exists'
             //sh "rm -rf ${WORKSPACE}/cd_config.yaml"
           }
     }
//# LOAD KUBECONFIG FILE FROM CREDENTIALS WE CREATED EARLIER
    stage('setup kubeconfig') {
          steps {
            withCredentials([file(credentialsId: 'standalone_config', variable: 'standalone_config')]) {
                sh "sudo cp \${standalone_config} ${WORKSPACE}/standalone_config"
            }
          }
    }
//# PUSH DOCKER IMAGE (LETS ASSUME YOU HAVE ROBOKEY SET IN DOCKERHUB)
//# PLEASE REPLACE THIS WITH YOUR DOCKER PUSH
     stage('push image to docker') {
      steps {
        withCredentials([string(credentialsId: 'robokey', variable: 'robokey')]) {
          sh '''
          echo "push image to docker"
          //sudo ./gradlew jar docker
          //sudo docker login -u="username" -p="password"
          //sudo ./gradlew jar dockerPushLatestTag dockerPushCommitTag 
        '''
        }
      }
     }
//# LETS EXTRACT IMAGE THAT WE JUST PUSHED AND DEPLOY TO QA 
//# REMEMBER WE USED {{IMAGE}} IN DEPLOYMENT.YAML AS TEMPLATE
//# MY ABOVE GRADLE TASK PUTS IMAGE IT CREATES IN FILE image.txt
//# WE WILL REPLACE {{IMAGE}} WITH IMAGE IN image.txt 
//# NOTE HERE WE ARE ASSUMING image.txt HOLDS THE IMAGE THERE MIGHT BE # OTHER WAYS YOU CAN STORE THE IMAGE AND REPLACE IT USING SHELL 
//# COMMANDS
    stage('deploy') {
          steps {
             sh '''
                echo Deploy
                //IMAGE=$(cat image.txt)
                //sed -i "s|{{image}}|$IMAGE|g" deployment.yaml
                //sudo kubectl --kubeconfig ${WORKSPACE}/cd_config config set-context --current --user=cd-sa
                //sudo kubectl apply -f deployment.yaml --kubeconfig ${WORKSPACE}/cd_config -n cd
                '''
          }
     }
     
//# CHECK IF ALL PODS ARE UP IN YOUR NAMESPACE
     stage('check if all pods are ready') {
           options {
             timeout(time: 10, unit: 'MINUTES')
            }

           steps {
              sh '''
                    echo 'check if all pods are ready'
                    //sudo kubectl --kubeconfig ${WORKSPACE}/cd_config rollout status deployment/cd-dep -n cd
                 '''
           }
      }
//# REMOVE KUBECONFIG AS IT IS NO LONGER NEEDED
     stage('remove kubeconfig file') {
       steps {
             echo 'remove kubeconfig file'
             //sh "sudo rm -rf ${WORKSPACE}/cd_config"
       }
     }
}
}
