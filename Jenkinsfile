pipeline {
    agent {
        docker { image 'circleci/node:10.16.0-stretch-browsers' }
    }  
    stages {
        stage('Install dependencies') {
            steps {
                sh 'npm install'
                stash includes: 'node_modules/', name: 'node_modules'
            }
        }
        stage('Lint') {
            steps {
                unstash 'node_modules'
                sh 'npm run lint'
            }
        }
        stage('Unit Test') {
            steps {
                unstash 'node_modules'
                sh 'npm run test'
                junit 'test-results/**/*.xml'
            }
        }
        stage('E2E Test') {
            steps {
                unstash 'node_modules'
                sh 'npm run e2e'
                junit 'coverage/**/*.xml'
            }
        }
        stage('Build') {
            steps {
                unstash 'node_modules'
                sh 'npm run build'
                stash includes: 'dist/', name: 'dist'
            }
        }
    }
}