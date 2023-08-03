pipeline {
   agent any
   stages {
      stage('Installation') {
         steps {
            bat 'npm install'
            bat 'npx playwright install'

         }
      }
      stage('e2e-tests') {
         parallel {            
            stage('chromium') {
               steps {
                  catchError(stageResult: 'FAILURE') {
                     bat  "NODE_OPTIONS='-r dd-trace/ci/init' DD_ENV=ci DD_SERVICE=my-javascript-app npx playwright test --project=chromium"
                  }
               }
            }
            stage('firefox') {
               steps {
                  catchError(stageResult: 'FAILURE') {
                     bat  "NODE_OPTIONS='-r dd-trace/ci/init' DD_ENV=ci DD_SERVICE=my-javascript-app npx playwright test --project=firefox"
                  }
               }
            }
         }
      }
      stage('Test reports') {
         steps {
            allure([includeProperties: false, jdk: '', reportBuildPolicy: 'ALWAYS', results: [[path: 'allure-results']]])
         }
      }
   }   
}
