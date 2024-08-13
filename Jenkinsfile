pipeline {
    agent any
    environment {
        NEW_VERSION = '1.1.0'
        GIT_CREDENTIALS = credentials('GitDanish')
    }
    parameters {
        string( name: 'VERSION', defaultValue: '', description: 'Dummy description of this version field' )
        choice( name: 'VERSION1', choices: ['1.1.0','1.1.1','1.1.2'], description: 'Choices of this version field' )
        booleanParam( name: 'executeTestBlock', defaultValue: true, description: 'Want to execute the Testing Block?' )
    }
    stages{
        stage('Dummy Stage') {
            steps {
                git 'https://github.com/hey-danish/jenkins_demo_project'
            }
        }
        stage("Stage: Building") {
            environment {
                AUTHOR = 'MD Abid'
            }
            steps {
                echo "Build number is ${currentBuild.number}"
                echo "Build is executing..${JOB_NAME}..${params.executeTestBlock}"
            }
            post {
                always {
                    echo "POST/Always -> ${currentBuild.currentResult} By ${AUTHOR}"
                }
                success {
                    echo "POST/Success -> Stage is successfully executed!"
                }
                failure {
                    echo "POST/Failed -> Stage is failed!"
                }
            }
        }
        stage("Stage: Testing") {
            when {
                expression {
                    params.executeTestBlock
                }
            }
            steps {
                echo "Testing is in progress!"
            }
        }
        stage("Stage: Deployment") { 
            when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
            }            
            steps {
                echo "Deployment is in progress!"
                withCredentials([usernamePassword(credentialsId: 'GitDanish', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                  sh 'echo ${PASSWORD}'
                  echo USERNAME
                  echo "username is ${USERNAME}"
                }
            }
        }
    }
  post {
    always {
      echo "Pipeline Completed with the status: ${currentBuild.currentResult}"
    }
    aborted {
        echo "Pipeline Execution was aborted"
    }
    failure {
        echo "Pipeline Execution was Failed"
    }
  }    
}
