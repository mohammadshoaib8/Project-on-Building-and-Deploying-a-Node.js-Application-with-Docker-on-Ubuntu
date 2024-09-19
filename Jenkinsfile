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
                sh 'sudo docker build . -t basanagoudapatil/nodo-todo-app-test:latest'
            }
        }
        stage('Test image') {
            steps {
                echo 'testing...'
                sh 'sudo docker inspect --type=image basanagoudapatil/nodo-todo-app-test:latest '
            }
        }
        
        stage('Push'){
            steps{
        	     sh "sudo docker login -u basanagoudapatil -p dckr_pat_OvN0lH_USJztUCkm0opyjz-yXNc"
                 sh 'sudo docker push basanagoudapatil/nodo-todo-app-test:latest'
            }
        }  
        stage('Deploy'){
            steps{
                echo 'deploying on another server'
                sh 'sudo docker stop nodetodoapp || true'
                sh 'sudo docker rm nodetodoapp || true'
                sh 'sudo docker run -d --name nodetodoapp basanagoudapatil/nodo-todo-app-test:latest'
                sh '''
                ssh -i "slavekey.pem" ubuntu@ec2-54-254-143-38.ap-southeast-1.compute.amazonaws.com <<EOF
                sudo docker login -u basanagoudapatil -p dckr_pat_OvN0lH_USJztUCkm0opyjz-yXNc
                sudo docker pull basanagoudapatil/nodo-todo-app-test:latest
                sudo docker stop nodetodoapp || true
                sudo docker rm nodetodoapp || true 
                sudo docker run -d --name nodetodoapp basanagoudapatil/nodo-todo-app-test:latest
                '''
            }
        }
    }
}
