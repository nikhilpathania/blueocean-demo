pipeline {
  agent any
  stages {
    stage('Build & Test') {
      steps {
        tool 'default_maven'
        sh '''export M2_HOME=/home/jenkins/tools/hudson.tasks.Maven_MavenInstallation/default_maven
export M2=$M2_HOME/bin
export PATH=$M2:$PATH

mvn -Dmaven.test.failure.ignore clean package'''
        stash(name: 'build-test-artifacts', includes: '**/target/surefire-reports/TEST-*.xml,target/*.jar')
      }
    }
    stage('Report & Publish') {
      steps {
        unstash 'build-test-artifacts'
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts(artifacts: 'target/*.jar', onlyIfSuccessful: true)
      }
    }
  }
}