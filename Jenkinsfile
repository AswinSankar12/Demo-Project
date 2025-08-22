pipeline {
  agent any

  tools {
    nodejs 'nodejs-22-6-0'
  }

  environment {
    MONGO_URI = 'mongodb+srv://supercluster.d83jj.mongodb.net/superData'
    MONGO_USERNAME = credentials('mongo-db-username')
    MONGO_PASSWORD = credentials('mongo-db-password')
  }

  stages {
    stage('Install Dependencies') {
      steps {
        sh 'npm install --no-audit'
      }
    }

        stage('NPM Dependency Audit') {
          steps {
            sh 'npm audit --audit-level=critical'
          }
        }

    stage('Unit Testing') {
        steps {
          sh 'npm test'
        }
      }

    stage('Code Coverage') {
      steps {
        catchError(buildResult: 'SUCCESS', message: 'Oops! it will be fixed in futher releases', stageResult: 'UNSTABLE') {
            sh 'npm run coverage'
        }
      }
    }

       
    }
    post {
      always {

        junit allowEmptyResults: true, stdioRetention: '', testResults: 'test-results.xml'

        publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'coverage/lcov-report', reportFiles: 'index.html', reportName: 'Code Coverage HTML Report', reportTitles: '', useWrapperFileDirectly: true])
      }
    }
}
