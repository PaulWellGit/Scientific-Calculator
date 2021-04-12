pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Start Build'
        sh 'mvn clean install'
        echo 'Build Complete'
      }
    }

    stage('Testing') {
      parallel {
        stage('Testing') {
          steps {
            sh 'mvn sonar:sonar -Dsonar.login=e94a30aab081fc288fa0dbd1e089d6d7c288459e -Dsonar.host.url=http://localhost:9000'
          }
        }

        stage('Print build number') {
          steps {
            echo "This is build number ${BUILD_ID}"
            sleep 20
          }
        }

      }
    }

  }
  tools {
    maven 'maven'
  }
  environment {
    TESTER = 'Paul'
    BUILD_ID = '1'
  }
}