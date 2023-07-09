
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
                   -Dsonar.host.url=http://3.6.39.148:9000 \
                   -Dsonar.projectKey=app-ci-cd '''
                  }     
             }
          }
    
 
        stage('OWASP Dependency Check') {
            steps {
                    dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                    }
              }

          stage('Docker Build') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'e36c3685-390b-4690-9421-35a46a52db35') {
                    sh "docker build -t  app-to-do:latest -f backend/Dockerfile . "
                    sh "docker tag app-to-do:latest sumitkumarbhagat/app-to-do:latest "
                    
                   }
                 }
               }
            
            }
          
           stage('Docker Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'e36c3685-390b-4690-9421-35a46a52db35') {
                    sh "docker push  sumitkumarbhagat/app-to-do:latest "
                 }
               }
            }
        }
        
        stage('trivy docker scan') {
            steps {
               sh " trivy image sumitkumarbhagat/app-to-do:latest "
            }
        }
        
        stage('Deploy to Docker') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'e36c3685-390b-4690-9421-35a46a52db35') {
                        sh " docker stop app-to-do " 
                        sh " docker rm app-to-do " 
                        sh " docker rmi app-to-do:latest || true  " 
                        sh "docker run -d --name app-to-do -p 4000:4000 sumitkumarbhagat/app-to-do:latest "
                 }
               }
            }
        }
     }
}
