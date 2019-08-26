pipeline {
  agent none
  stages {
    stage('Install dependencies') {
      agent {
        docker 'circleci/node:10.16.0-stretch-browsers'
      }
      steps {
        sh 'npm install'
        stash includes: 'node_modules/', name: 'node_modules'
      }
    }
    stage('Lint') {
      agent {
        docker 'circleci/node:10.16.0-stretch-browsers'
      }
      steps {
        unstash 'node_modules'
        sh 'npm run lint'
      }
    }
    stage('Unit Test') {
      agent {
        docker 'circleci/node:10.16.0-stretch-browsers'
      }
      steps {
        unstash 'node_modules'
        sh 'npm run test'
        junit 'test-results/**/*.xml'
      }
    }
    stage('E2E Test') {
      agent {
        docker 'circleci/node:10.16.0-stretch-browsers'
      }
      steps {
        unstash 'node_modules'
        sh 'npm run e2e'
        junit 'coverage/**/*.xml'
      }
    }
    stage('Compile') {
      agent {
        docker 'circleci/node:10.16.0-stretch-browsers'
      }
      steps {
        unstash 'node_modules'
        sh 'npm run build'
        stash includes: 'dist/', name: 'dist'
      }
    }
  }
}