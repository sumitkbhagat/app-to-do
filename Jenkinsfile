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
                   -Dsonar.host.url=http://13.234.232.137:9000 \
                   -Dsonar.projectKey=app-ci-cd '''
                  }     
             }
          }
    }
 }
            
             
         
     

             stage('OWASP Dependency Check') {
            steps {
                    dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
             }
         }
     }
}
