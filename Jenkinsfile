pipeline {
    agent any
    triggers {
        // Tells Jenkins to listen for incoming GitHub push webhooks
        githubPush()
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Test Webhook') {
            steps {
                echo '🎉 Success! The GitHub webhook successfully triggered this build!'
                sh 'node index.js'
            }
        }
    }
}

