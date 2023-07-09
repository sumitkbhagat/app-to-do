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
                   -Dsonar.host.url=http://65.0.185.65:9000 \
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
                    sh "docker build -t  todoapp:latest -f docker/Dockerfile . "
                    sh "docker tag todoapp:latest username/todoapp:latest "
                 }
               }
            }
        }

        stage('Docker Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'e36c3685-390b-4690-9421-35a46a52db35') {
                    sh "docker push  username/todoapp:latest "
                 }
               }
            }
        }
        stage('trivy') {
            steps {
               sh " trivy username/todoapp:latest"
            }
        }
		stage('Deploy to Docker') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'e36c3685-390b-4690-9421-35a46a52db35') {
                    sh "docker run -d --name to-do-app -p 4000:4000 username/todoapp:latest "
                 }
               }
            }
        }

    }
}
        
