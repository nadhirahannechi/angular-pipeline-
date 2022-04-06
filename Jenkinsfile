pipeline {
    agent any
 stages {
      agent {
              docker {
                image('trion/ng-cli-karma:1.2.1')
              }
      }
          
    stage('Checkout') {
        // disable to recycle workspace data to save time/bandwidth
        steps {
        deleteDir()
        checkout scm
        }
    }
     
      stage('NPM Install') {
          // silent warn messages
          withEnv(["NPM_CONFIG_LOGLEVEL=warn"]) {
              sh 'npm install'
          }
      }

      stage('Test') {
          withEnv(["CHROME_BIN=/usr/bin/chromium-browser"]) {
            sh 'ng test --progress=false --watch false'
          }
          // add a reporter that creates JUnit XML reports
          junit '**/test-results.xml'
      }

      stage('Lint') {
          // add a reporter that creates JUnit XML reports
          sh 'ng lint'
      }
        
      stage('Build') {
          milestone()
          sh 'ng build --prod --aot --sm --progress=false'
      }
    }
    //end docker
    stages {

        stage('Archive') {
            // archive the build artifact
            sh 'tar -cvzf dist.tar.gz --strip-components=1 dist'
            archive 'dist.tar.gz'
        }

        stage('Deploy') {
            milestone()
            echo "Deploying..."
        }
  }
}
