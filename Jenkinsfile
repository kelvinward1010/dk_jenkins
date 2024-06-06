pipeline {
    agent { label 'buil-in'}

    tools {
        nodejs 'node_v20'
    }

    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}