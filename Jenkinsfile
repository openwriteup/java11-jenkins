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

				withCredentials([usernamePassword(credentialsId: 'dockerlogin', passwordVariable: 'password', usernameVariable: 'username')])
				{
				
        sh '''echo $password |docker login -u $username --password-stdin
docker push  amitow/testjava'''
}
  
            }
        }

    }
}
