pipeline{

    agent any
        
        tools{
            maven 'maven'
            }

        stages{

            stage("build"){
                steps{
                    git 'https://github.com/jglick/simple-maven-project-with-tests.git'
                    bat "mvn -Dmaven.test.failure.ignore=true clean package"
                 }
                 post
                 {
                     success
                     {
                          junit '**/target/surefire-reports/TEST-*.xml'
                          archiveArtifacts 'target/*.jar'
                     }
                  }
             }
           
            stage("deploy to QA"){
                steps{
                    echo("deploy to QA")
                 }
             }

            stage("Regression Automation Test"){
                steps{
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        git 'https://github.com/PradeepR-101/NAL_SeleniumJavaFramework'
                        bat "mvn clean test -Dsurefire.suiteXmlFiles=src/main/resources/testrunners/testng_regression.xml"
                 }
             }

            stage("publish allure report"){
                steps{
                    script {
                        allure([
                            includeProperties: false,
                            jdk: ''
                            properties: []
                            reportBuildPolicy: 'ALWAYS'
                            results: [[path: '/allure-results']]
                        ]}
                 }
             }
          
             stage("deploy to Stage"){
                steps{
                    echo("deploy to Stage")
                 }
             }

            stage("Sanity Automation Test"){
                steps{
                    catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                        git 'https://github.com/PradeepR-101/NAL_SeleniumJavaFramework'
                        bat "mvn clean test -Dsurefire.suiteXmlFiles=src/main/resources/testrunners/testng_sanity.xml"
                 }
             }

            stage("publish allure report"){
                steps{
                    script {
                        allure([
                            includeProperties: false,
                            jdk: ''
                            properties: []
                            reportBuildPolicy: 'ALWAYS'
                            results: [[path: '/allure-results']]
                        ]}
                 }
             }
            
       }
}