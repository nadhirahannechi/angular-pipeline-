pipeline {
 agent {
        docker {
            image 'trion/ng-cli-karma:1.2.1' 
        }
    }
 options {
    buildDiscarder(logRotator(numToKeepStr: '30', artifactNumToKeepStr: '30'))
  }

    stages {    
      
         stage('Checkout') {
             agent any
             // disable to recycle workspace data to save time/bandwidth
             steps {
              deleteDir()
              checkout scm
            }
         }

          stage('NPM Install') {
              // silent warn messages
               steps {
                  sh 'npm install'
              
               }
          }

          stage('Test') {
               steps {
               script{
              withEnv(["CHROME_BIN=/usr/bin/chromium-browser"]) {
                sh 'ng test --progress=false --watch false'
              }
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
