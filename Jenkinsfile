pipeline {
  agent any
  stages {
    stage ('Build') {
      steps {
        echo 'Running build automation'
        sh './gradlew build --no-daemon'
        archiveArtifacts artifacts: dist/trainSchedule.zip
      }
      stage ('Build Docker Image') {
        when {
          branch 'master'
        }
        steps {
         echo 'Running docker build'
         app = docker.build("volovik/train-schedule")
          app.inside {
              sh 'echo $(curl localhost:8000)'
          }
      }  
      stage ('Push Docker Image') {
        when {
          branch 'master'
        }
        steps {
          script {
            echo 'Running docker build'
            app = docker.build("volovik/train-schedule")
            app.inside {
              sh 'echo $(curl localhost:8000)'
                }  
              }
        }  
      stage ('deploy') {
        steps {
          script {
            docker.withRegistry('https://hub.docker.com', 'docker_hub_login') {
              app.push = ("${env.BUILD_NUMBER}")
              app.push = ("latest")
            }
        }
       }
    }
  }
}
