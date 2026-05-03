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
  stage('setup trivy') {
            steps {
         sh '''   sudo apt-get install wget gnupg
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y'''
            }
        }	

		  stage('scan') {
            steps {
            sh ' trivy image --format json --output result.json  --severity HIGH,CRITICAL  amitow/testjava'
				archiveArtifacts artifacts: 'result.json', followSymlinks: false
				slackUploadFile channel: 'jenkinswp', credentialId: 'a47de10e-69d4-4e86-9d35-4d77ab3a1580', filePath: 'result.json'
            }
        }
 stage('mailing') {
            steps {
				emailext attachmentsPattern: 'report.json', body: 'PFA', subject: 'Hi Trivy report', to: 'amit23comp@gmail.com'
	mail bcc: '', body: 'echo jobstatus ', cc: '', from: 'openwriteup@gmail.com', replyTo: '', subject: 'Pipeline status', to: 'openwriteup@gmail.com'
        }
 }
    }
}
