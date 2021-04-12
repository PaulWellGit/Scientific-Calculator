pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package checkstyle:checkstyle findbugs:findbugs pmd:pmd'
      }
    }

  }
}