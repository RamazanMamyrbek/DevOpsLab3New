pipeline {
  agent none

  options {
    skipStagesAfterUnstable()
  }

  stages {
    stage("Build & Test in container") {
      agent {
        docker {
          image 'openjdk:11.0.5-slim'
          args '-v $HOME/.m2:/root/.m2'
        }
      }
      stages {
        stage('Checkout') {
          steps {
            checkout scm
          }
        }
        stage('Compile') {
          steps {
            sh './mvnw compile'
          }
        }
        stage('Test') {
          steps {
            sh './mvnw test'
            junit '**/target/surefire-reports/TEST-*.xml'
          }
        }
        stage('Package') {
          steps {
            sh './mvnw package -DskipTests'
          }
        }
      }
    }
  }
}
