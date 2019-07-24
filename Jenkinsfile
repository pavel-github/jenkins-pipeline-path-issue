pipeline {
  agent { label 'nodejs' }

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
        }
        withEnv(['PATH+MAVEN=/opt/jenkins/tools/hudson.tasks.Maven_MavenInstallation/3.6.0/bin']) {
          sh 'echo $PATH'
          snykSecurity monitorProjectOnBuild: false,
                       snykInstallation: 'snyk@latest',
                       snykTokenId: 'my-snyk-api-token',
                       failOnIssues: true
        } 
      }
    }
    
    stage('Build') {
      steps {
        withMaven(maven: '3.6.0') {
          sh 'mvn clean package'
        }
      }
    }
  }
}
