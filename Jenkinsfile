pipeline {
  agent {
    node {
      label 'slave'
    }

  }
  stages {
    stage('git checkout') {
      steps {
        git(url: 'https://github.com/Ram8319/mavenrepo.git', branch: 'master', credentialsId: 'Git-token')
      }
    }

    stage('maven build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('sonar checks') {
      steps {
        withSonarQubeEnv(installationName: 'sonar', credentialsId: 'sonar-token', envOnly: true) {
          sh 'mvn sonar:sonar'
        }

      }
    }

  }
}