pipeline {
    agent any
    
    stages{
        stage('Checkout'){
            steps{
                git url: 'https://github.com/Basanagoudapatil02/Project-on-Building-and-Deploying-a-Node.js-Application-with-Docker-on-Ubuntu.git', branch: 'master'
            }
        }
        stage('Build'){
            steps{
                sh 'sudo docker build . -t msshoaib2255457/myapp:latest'
            }
        }
        stage('Test image') {
            steps {
                echo 'testing...'
                sh 'sudo docker inspect --type=image msshoaib2255457/myapp:latest '
            }
        }
        
        stage('Push'){
            steps{
        	     sh "sudo docker login -u ms.shoaib2255@gmail.com -p 177y1a0464"
                 sh 'sudo docker push msshoaib2255457/myapp:latest'
            }
        }  
        stage('Deploy'){
            steps{
                echo 'deploying on another server'
                sh 'sudo docker stop nodetodoapp || true'
                sh 'sudo docker rm nodetodoapp || true'
                sh 'sudo docker run -d --name nodetodoapp msshoaib2255457/myapp:latest'
                sh '''
                ssh -i "slavekey.pem" ubuntu@ec2-54-254-143-38.ap-southeast-1.compute.amazonaws.com <<EOF
                sudo docker login -u ms.shoaib2255@gmail.com -p 177y1a0464
                sudo docker pull msshoaib2255457/myapp:latest
                sudo docker stop nodetodoapp || true
                sudo docker rm nodetodoapp || true 
                sudo docker run -d --name nodetodoapp msshoaib2255457/myapp:latest
                '''
            }
        }
    }
}
