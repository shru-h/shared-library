pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/my-username/my-repo.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv('my-sonarqube-server') {
          sh 'mvn sonar:sonar'
        }
      }
    }
    stage('Quality Gate') {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          def qg = waitForQualityGate()
          if (qg.status != 'OK') {
            error "Pipeline aborted due to quality gate failure: ${qg.status}"
          }
        }
      }
    }
  }
}
