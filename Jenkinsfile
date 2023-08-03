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
                     bat  "npx playwright test --project=chromium"
                  }
               }
            }
            stage('firefox') {
               steps {
                  catchError(stageResult: 'FAILURE') {
                     bat  "npx playwright test --project=firefox"
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
