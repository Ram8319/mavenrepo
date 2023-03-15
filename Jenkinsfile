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
      parallel {
        stage('sonar checks') {
          steps {
            withSonarQubeEnv(installationName: 'sonar', credentialsId: 'sonar-token', envOnly: true) {
              sh 'mvn sonar:sonar'
            }

          }
        }

        stage('tomcat deploy') {
          steps {
            sh 'sudo scp /home/ubuntu/workspace/mavenrepo_master/target/studentapp-2.5-SNAPSHOT.war 172.31.41.105:/opt/tomcat/webapps'
          }
        }

      }
    }

  }
}