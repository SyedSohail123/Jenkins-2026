pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // This checks out the code from the Git repository that triggers it
                checkout scm
            }
        }
        stage('Test Trigger') {
            steps {
                echo 'Success! The pipeline was successfully triggered via GitHub Webhook.'
            }
        }
    }
}

