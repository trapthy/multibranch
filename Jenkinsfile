pipeline {

    agent {
        label {
          label "gp-agent"
             }
          }
    environment{
        branch = env.BRANCH_NAME

    }


    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: $branch , 
                    userRemoteConfigs: [[url: 'https://github.com/trapthy/multibranch.git']]
                ])
            }
        }

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
