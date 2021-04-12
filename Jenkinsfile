pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -f Calculator/pom.xml -B -DskipTests clean package checkstyle:checkstyle findbugs:findbugs pmd:pmd'
      }
    }

    stage('Test') {
      post {
        always {
          junit 'Calculator/target/surefire-reports/*.xml'
        }

      }
      steps {
        sh 'mvn -f Calculator/pom.xml test'
      }
    }

  }
}