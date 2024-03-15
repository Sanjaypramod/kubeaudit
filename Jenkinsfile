pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'master', url: 'https://github.com/Sanjaypramod/kubeaudit.git'
            }
        }

        stage('Authenticate with EKS') {
            environment {
                AWS_DEFAULT_REGION = 'us-west-2'
            }
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    credentialsId: 'aws-creds',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                ]]) {
                    sh 'aws eks update-kubeconfig --name sanjay-cluster --region us-west-2'
                    sh 'kubectl apply -f manifest.yaml'
                    sh 'kubectl get node'
                    sh 'kubectl get pods -A'
                    // sh 'sudo wget https://golang.org/dl/go1.17.2.linux-amd64.tar.gz'
                    // sh 'sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.2.linux-amd64.tar.gz'
                    // sh 'sudo export PATH=$PATH:/usr/local/go/bin'
                    // sh 'sudo go version'
                    // sh 'sudo apt install kubeaudit'
                    // sh 'kubeaudit version'
                    // sh 'kubeaudit all'
                    sh 'kubeaudit all -f /var/lib/jenkins/kubeaudit/manifest.yaml > audit12.log' 
                    sh 'pwd'
                    sh 'kubeaudit all -f /var/lib/jenkins/kubeaudit/fixed-manifest.yaml > audit.log' 
                    // sh "ls"
                    // sh 'kubeaudit version'
                    sh "whoami"
                    sh "echo $PATH"
                }
            }
        }   
    }
}

