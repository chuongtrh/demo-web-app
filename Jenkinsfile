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
        }
        stage('E2E Test') {
            steps {
                unstash 'node_modules'
                sh 'npm run e2e'
                cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/cobertura.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
            }
        }
        stage('Build') {
            steps {
                unstash 'node_modules'
                sh 'npm run build'
                stash includes: 'dist/', name: 'dist'
            }
        }
        stage('Upload S3 DEV') {
            steps {
                unstash 'dist'
                withAWS(region:'ap-southeast-1',credentials:'aws-dev-ops') {
                    s3Upload(bucket:"demo-web-app-dev", workingDir:'dist', includePathPattern:'**/*');
                }
            }
        }
        stage('Sanity check') {
            steps {
                input "Do you want to deploy on STAGING?"
            }
        }
        stage('Upload S3 STAGING') {
            steps {
                unstash 'dist'
                withAWS(region:'ap-southeast-1',credentials:'aws-dev-ops') {
                    s3Upload(bucket:"demo-web-app-staging", workingDir:'dist', includePathPattern:'**/*');
                }
            }
        }
    }
}