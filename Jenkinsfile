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

    stage('nexus repository') {
      steps {
        nexusArtifactUploader(nexusVersion: 'nexus3', protocol: 'http', nexusUrl: '172.31.4.22', groupId: 'com.jdevs', version: '2.5', repository: 'http://13.232.19.43:8081/repository/maven-snapshots/', credentialsId: 'nexus-cred')
        sh 'mvn deploy'
      }
    }

  }
}