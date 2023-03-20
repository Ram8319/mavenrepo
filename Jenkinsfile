pipeline {
  agent {
    node {
      label 'slave'
    }

  }
  stages {
    stage('Gitcheckout') {
      steps {
        git(url: 'https://github.com/Ram8319/mavenrepo.git', branch: 'master', credentialsId: 'Git-token')
      }
    }

    stage('Maven-UnitTest') {
      steps {
        sh 'mvn test'
      }
    }

    stage('Maven-warfile') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Sonar-checks') {
      steps {
        withSonarQubeEnv(installationName: 'sonar', credentialsId: 'sonar-token', envOnly: true) {
          sh 'mvn sonar:sonar'
        }

      }
    }

    stage('Deploy-artifact-to NexusRepo') {
      steps {
        sh 'mvn deploy'
      }
    }

  }
}