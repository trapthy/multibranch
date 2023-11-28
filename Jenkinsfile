pipeline {

    agent {
        label {
          label "gp-agent"
             }
          }
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

        // stage('Code Checkout') {
        //         steps {
        //          git branch: "${env.BRANCH_NAME}", credentialsId: 'trapthygit', url: 'https://github.com/trapthy/multibranch.git'
        //     }
        // }

        stage(' Unit Testing') {
            when {
                branch "develop"
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

        stage('Build Deploy Code') {
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

    }   
}
