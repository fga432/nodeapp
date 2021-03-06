pipeline {
  agent any
    
  tools {
    // In Global tools configuration, install Node configured as "nodejs 13"
    nodejs "nodejs 13"
  }
    
  stages {
        
    stage('Cloning Git') {
      steps {
        git 'https://github.com/ualcnsa/nodeapp'
      }
    }
        
    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }
     
    stage('Test') {
      steps {
         sh 'npm run coverage-jenkins'
      }
      post { 
        success {
          junit '**/test*.xml'
          publishHTML target: [
            allowMissing         : false,
            alwaysLinkToLastBuild: false,
            keepAll             : true,
            reportDir            : './coverage/',
            reportFiles          : 'index.html',
            reportName           : 'Coverage Report'
          ]
        }
      }    
    }

    stage('Analysis'){
       steps{
          sh 'npm run lint -s'
       }
       post {
         success {
            recordIssues enabledForFailure: true, tool: checkStyle(), filters: [includeFile('coverage/eslint-result.xml')] 
         }
      }
    }
  }
}