pipeline {
 agent {
        docker {
            image 'node:10.0.0' 
        }
    }
   environment {
        HOME = '.'
    }
    stages {    
         stage('Checkout') {
            // disable to recycle workspace data to save time/bandwidth
            steps {
            deleteDir()
            checkout scm
            }
        }

          stage('NPM Install') {
              // silent warn messages
               steps {
                 sh 'rm -rf node_modules'
                  sh 'npm install'
              
               }
          }

          stage('Test') {
               steps {
              withEnv(["CHROME_BIN=/usr/bin/chromium-browser"]) {
                sh 'ng test --progress=false --watch false'
              }
              // add a reporter that creates JUnit XML reports
              junit '**/test-results.xml'
          }
          }

          stage('Lint') {
            steps {   // add a reporter that creates JUnit XML reports
              sh 'ng lint'
          }
          }

          stage('Build') {
              steps {
                    sh 'ng build --prod --aot --sm --progress=false'
              }
          }

        stage('Archive') {
            agent any
            // archive the build artifact
             steps {
            sh 'tar -cvzf dist.tar.gz --strip-components=1 dist'
            archive 'dist.tar.gz'
             }
        }

        stage('Deploy') {
            agent any
             steps {
            echo "Deploying..."
             }
        }
    }
  }
