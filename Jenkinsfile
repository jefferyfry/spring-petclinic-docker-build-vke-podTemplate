
pipeline {
  agent {
    kubernetes {
        label 'docker-build-pod'
        yamlFile 'podTemplate/spring-petclinic-docker-build.yaml'
    }
  }
  stages {
    stage('Maven Install') {
      steps {
        container('maven') {
          sh 'mvn clean install'
        }
      }
    }
    stage('Docker Build') {
      steps {
        container('docker'){
          sh 'docker build -t jefferyfry/spring-petclinic:latest .'
        }
      }
    }
    stage('Docker Push') {
      steps {
        container('docker'){
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
            sh 'docker push jefferyfry/spring-petclinic:latest'
          }
        }
      }
    }
    stage('Deploy to Staging Server') {
      steps {
        container('vke-kubectl'){
          withCredentials([string(credentialsId: 'vkeToken', variable: 'token')]) {
            sh "vke account login -t 98c89a30-8017-4ba4-a981-2c873475d841 -r ${env.token}"
            sh '''
                 vke cluster merge-kubectl-auth cloudbees-core-vke-priv
                 kubectl delete namespace spring-petclinic-docker-build || true
                 sleep 5
                 kubectl create namespace spring-petclinic-docker-build
                 kubectl run spring-petclinic-docker-build --image=jefferyfry/spring-petclinic:latest --port 8080 --namespace spring-petclinic-docker-build
                 kubectl expose deployment spring-petclinic-docker-build --type=LoadBalancer --port 8092 --target-port 8080 --namespace spring-petclinic-docker-build
                 while [ -z "$url" ]; do url=$(kubectl describe service spring-petclinic-docker-build --namespace spring-petclinic-docker-build | grep spring-petclinic-docker-build. | awk -F: \'{gsub (\" \", \"\", $0); print \"http://\"$2\":8092\"}\'); sleep 2; done

                 echo "$url"
            '''
          }
        }
      }
    }
  }
}