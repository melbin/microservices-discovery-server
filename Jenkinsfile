pipeline {
  agent any
  stages {
    stage('Checkout repository') {
      parallel {
        stage('Checkout repository') {
          steps {
            git(url: 'https://github.com/melbin/microservices-discovery-server.git', branch: 'master')
          }
        }
        stage('Maven Config') {
          agent any
          steps {
            withMaven(maven: 'Maven 3.1.1') {
              sh 'sh \'mvn package\''
            }

          }
        }
      }
    }
    stage('Compile Stage') {
      steps {
        sh '''echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
bat """wmic Path win32_process Where "CommandLine Like \'%%discovery-server-0.0.1-SNAPSHOT.jar%%\'" Call Terminate"""
bat \'mvn package\''''
      }
    }
    stage('Testing Stage') {
      steps {
        sh 'bat \'mvn test\''
      }
    }
    stage('Deployment Stage') {
      steps {
        sh 'bat """start /B java -jar target\\\\discovery-server-0.0.1-SNAPSHOT.jar"""'
      }
    }
  }
}