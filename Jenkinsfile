pipeline {
    agent any //{ label 'buil-in'}

    tools {
        nodejs 'node_v20'
    }

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'github-123', poll: false, url: 'https://github.com/kelvinward1010/dk_jenkins.git' //checkout scm
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