pipeline {
  agent {
    docker {
      image 'circleci/node:10-stretch-browsers'
    }

  }
  stages {
    stage('NPM Install') {
      steps {
        sh 'npm install'
      }
    }
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('sq-cloud') {
          sh 'printenv | sort'
          sh "npm run sonar-scanner -- -Dsonar.login=${env.SONAR_AUTH_TOKEN}"
//          sh "./gradlew --info sonarqube " +
//            "-Dsonar.projectKey=nbrinks_spring-boot-demo " +
//            "-Dsonar.organization=nbrinks-github " +
//            "-Dsonar.host.url=${env.SONAR_HOST_URL} " +
//            "-Dsonar.login=${env.SONAR_AUTH_TOKEN} " +
//            "-Dsonar.pullrequest.key=${env.CHANGE_ID} " +
//            "-Dsonar.pullrequest.branch=${env.CHANGE_BRANCH} " +
//            "-Dsonar.pullrequest.base=${env.CHANGE_TARGET}"
        }
      }
    }
    stage('SonarQube Quality Gate') {
      options { timeout(time: 30, unit: 'MINUTES') }
      steps {
        sh 'printenv | sort'
        waitForQualityGate(abortPipeline: true)
      }
    }
  }
}

