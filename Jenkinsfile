pipeline {
    agent any
    
   environment{
    SCANNER_HOME= tool 'sonar-scanner'
}
  stages {
        stage('git-checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/sumitkbhagat/app-to-do.git'
            }
         }
     stage("Sonarqube Analysis ") {
      steps{
                 withSonarQubeEnv('sonar-scanner') {
                  sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=app-ci-cd \
                       -Dsonar.java.binaries=. \
                -Dsonar.host.url=http://13.235.238.178:9000 \
                  -Dsonar.projectKey=app-ci-cd '''
               }
            }
         }
    }  
}
 
     

