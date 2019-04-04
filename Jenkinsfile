pipeline {
  agent { label 'nodejs' }

  environment {
    PATH = "/opt/jenkins/tools/hudson.tasks.Maven_MavenInstallation/3.6.0/bin:$PATH"
  }

  stages {
    stage('Checkout') {
      steps {
        deleteDir()
        git 'https://github.com/pavel-github/jenkins-pipeline-path-issue'
      }
    }

    stage('Security') {
      steps {
        sh 'echo $PATH'
        withMaven(maven: '3.6.0') {
          sh 'mvn clean test'
          snykSecurity monitorProjectOnBuild: false,
                       snykInstallation: 'snyk@latest',
                       snykTokenId: 'my-snyk-api-token',
                       failOnIssues: true
        }
      }
    }
  }
}
