pipeline {
    agent any
 
    stages {
        stage('SCM') {
            steps {
               git branch: 'master', url: 'https://github.com/vicente19821/aforo'
            }
        }
        
      stage ('Build Net6.0') {
           steps {
        
            sh 'dotnet restore';
            sh 'dotnet build' ;
            sh 'dotnet test';
         
           }
       }
       
        stage ("Docker Build") {
           
           steps{
             // docker login  
             sh 'docker build -t vicente1982/afoto .' ;
            sh  'docker push vicente1982/afoto';  
           }
       }   
     stage ("Deploy AKS") {
           steps {
           sh 'az login --service-principal --username 91ff72d0-e273-4642-aed7-94b6a5ceb1a9  --password tbN8Q~izQextZOBU-CVCZz6I2dtH0E2-XVCacavr  --tenant 1014e0df-cb4f-4c6e-8e59-001832ca00fa';
            sh 'az account set --subscription 7a349d44-09aa-4e98-83e8-081994570721 & kubectl config get-contexts --kubeconfig=${KUBE_PATH_CONFIG}';
            sh 'kubectl apply -f k8s.yml --kubeconfig=${KUBE_PATH_CONFIG}';
            sh  'kubectl rollout restart deployment app-deployment --kubeconfig=${KUBE_PATH_CONFIG}';
           }
       }
       
    }
}
