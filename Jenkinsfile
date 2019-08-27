pipeline {
    agent {
        docker { image 'samtran/node:10.16.0-browsers' }
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
            post {
                always {
                    step([$class: 'Test Results', coberturaReportFile: 'test-results/**/*.xml'])
                }
            }
        }
        stage('E2E Test') {
            steps {
                unstash 'node_modules'
                sh 'npm run e2e'
            }
            post {
                always {
                    step([$class: 'Coverage Report', coberturaReportFile: 'coverage/**/*.xml'])
                }
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