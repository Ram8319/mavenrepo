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
      parallel {
        stage('Sonar-checks') {
          steps {
            withSonarQubeEnv(installationName: 'sonar', credentialsId: 'sonar-token', envOnly: true) {
              sh 'mvn sonar:sonar'
            }

          }
        }

        stage('Deploy-to-Tomcat') {
          steps {
            sh 'sudo scp /home/ubuntu/workspace/mavenrepo_master/target/studentapp-2.5-SNAPSHOT.war 172.31.3.101:/opt/tomcat/webapps'
          }
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