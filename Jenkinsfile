pipeline {
  agent {
    dockerfile {
      filename 'Dockerfile'
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
        sh './gradlew test || exit 0'
        junit 'app/build/test-results/test*/*xml'
        sh './gradlew jacocoTestReport || exit 0'
        jacoco()
      }
    }

    stage('Static Tests') {
      steps {
        sh './gradlew checkstyle || exit 0'
        sh './gradlew pmd || echo 0'
        sh './gradlew findbugs || echo 0'
        recordIssues(enabledForFailure: true, tool: checkStyle(pattern: 'app/build/reports/checkstyle/*.xml'), sourceCodeEncoding: 'UTF-8')
        recordIssues(enabledForFailure: true, tool: pmdParser(pattern: 'app/build/reports/pmd/*.xml'), sourceCodeEncoding: 'UTF-8')
        recordIssues(enabledForFailure: true, tool: spotBugs(pattern: 'app/build/reports/findbugs/*xml'), sourceCodeEncoding: 'UTF-8')
      }
    }

  }
  environment {
    HOME = '/root/'
    _JAVA_OPTIONS = '-Duser.home=/root/'
  }
}