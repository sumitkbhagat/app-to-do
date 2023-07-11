
pipeline {
    agent any
   
   

    stages {
        stage('git-checkout') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/sumitkbhagat/app-to-do.git'
            }
        }
            
        }
     }

