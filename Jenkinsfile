pipeline {
  agent {
    docker {
      args '-v /jenkins/.gradle:/jenkins/.gradle'
      image 'gradle:latest'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh './gradlew :build'
      }
    }
    stage('Test') {
      steps {
        sh './gradlew :test'
        junit 'build/test-results/test/*xml'
        sh './gradlew jacocoTestReport'
        jacoco()
      }
    }
    stage('Static Tests') {
      steps {
        sh './gradlew checkstyle'
        sh './gradlew pmd || echo 0'
        sh './gradlew findbugs || echo 0'
        recordIssues enabledForFailure: true, tool: checkStyle(pattern: 'app/build/reports/checkstyle/*.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'app/build/reports/pmd/*.xml'), sourceCodeEncoding: 'UTF-8'
        recordIssues enabledForFailure: true, tool: spotBugs(pattern: 'app/build/reports/findbugs/*xml'), sourceCodeEncoding: 'UTF-8'
      }
    }
  }
  environment {
    GRADLE_USER_HOME = '/jenkins/.gradle'
  }
}
