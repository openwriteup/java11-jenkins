pipeline {
    agent any

    stages {
        stage('clone') {
            steps {
               git 'https://github.com/openwriteup/java11-jenkins.git'
            }
        }
	  stage('build') {
            steps {
              sh 'ssudo docker build -t amitow/testjava -f dockerfile .'
            }
        }

    }
}
