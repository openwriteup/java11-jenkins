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
            sh 'docker build -t amitow/testjava -f dockerfile .'
            }
        }
stage('push') {
            steps {
				withCredentials([usernameColonPassword(credentialsId: 'dockerlogin', variable: 'mytoken')]) {
        sh '''echo $mytoken |docker login -u amitow --password-stdin
docker push  amitow/testjava'''
}
  
            }
        }

    }
}
