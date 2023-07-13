pipeline {
   agent any
   parameters {
      choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
      booleanParam(name: 'executeTests', defaultValue: true, description: '')
   }
   stages {
      stage("Checkout") {
         steps {
            checkout scm
         }
      }
      stage("Build") {
         steps {
            sh 'docker-compose build web'
         }
      }
      stage("Tag and Push") {
         steps {
            withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'DOCKER_USER_PASSWORD', usernameVariable: 'DOCKER_USER_ID')]) {
               sh "docker tag jenkins-pipeline_web:latest hjju98/jenkins-app:${env.BUILD_NUMBER}"
               sh "docker login -u hjju98 -p time0606!"
               sh "docker push hjju98/jenkins-app:${env.BUILD_NUMBER}"
            }
         }
      }
      stage("Deploy") {
         steps {
            sh "docker-compose up -d"
         }
      }
   }
}
