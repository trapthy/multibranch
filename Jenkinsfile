pipeline {

    agent any
    // {
    //     label {
    //       label "gp-agent"
    //          }
    //       }
    environment{

       branch_name = "${env.BRANCH_NAME}"
    //     branch = "${env.BRANCH_NAME.split("/")[1]}"


     }


    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
              
                echo "$branch_name"
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
                steps {
                 git branch: "$branch_name", credentialsId: 'trapthygit', url: 'https://github.com/trapthy/multibranch.git'
            }
        }

        stage(' Unit Testing') {
            when {
                branch "feature*"
            }
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
             when {
                branch "develop"
            }
            steps {
                sh """
                echo "Running Code Analysis"
                """
            }
        }

         stage('Trigger notification for Tag creation') {
             when {
                branch "develop"
            }
            steps {
                script{
                        emailext mimeType: 'text/html', 
                        body: "Please go to below link provided to approve or reject the deployment  <br><br> <a href=\" ${BUILD_URL}/input\">Approve or Reject tag creation?</a> ",
                        subject: "[JENKINS] create tag? ",
                        to: "trapthyshetty@gmail.com"
                    
                 
                        timeout(time: 2, unit: "DAYS") {
	                     input message: "Do you want to approve the tag creation on Integration?", ok: 'Yes'
                            
                        }
                        }
                }
            }
         stage('Fetch Git commitID for this build')  { 
          steps {
                script {
                  
                    git branch: "${env.BRANCH_NAME}", credentialsId: "${trapthygit}", url: "https://github.com/trapthy/multibranch.git"
                    env.GIT_COMMIT = sh(script: 'git rev-parse HEAD', returnStdout: true)?.trim()
                    echo "Commit ID for this Build is : $env.GIT_COMMIT"
                 
                }
            }
        }
        stage('Tag creation') {
             when {
                branch "develop"
            }
            steps {
               
                        sh """
                        echo "************Tag creation"
                        
                        git tag -a ver1.0 env.GIT_COMMIT -m "create tag"
                        """  
                            
                        
                        }
                }
            }
        stage('Build Deploy Code to INT') {
            when {
                branch 'develop'
            }
            steps {
                sh """
                echo "Building Artifact"
                """

                sh """
                echo "Deploying Code"
                """
            }
        }

          stage('Build Deploy Code to UAT') {
              when{
                  anyOf{
                      buildingTag()
                      branch "release*"
                  }
              }
            // when {
                   
            //         tag pattern: "version*" , comparator: "REGEXP"
            //     }
            // }
            steps {
                sh """
                echo "Building Artifact"
                """

                sh """
                echo "Deploying Code"
                """
            }
        }

    }   
}
