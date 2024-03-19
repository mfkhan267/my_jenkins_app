pipeline {
    agent any
    environment {
        AZURE_TENANT_ID = "61952628-b9f1-4dbe-9ea2-311ffb0da156"
        AZURE_SUBSCRIPTION_ID = "c8105223-fff8-4acf-9281-4171ea50d6ac"
    }

    stages {
        stage('git checkout') {
            steps {
              checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mfkhan267/my_jenkins_app.git']]])
              sh "pwd"
              sh "ls -ltr"
              dir("${env.WORKSPACE}/container"){
                    sh "pwd"
                        }
                    }
                              }
        stage('build docker image'){
            steps{
    	        dir("${env.WORKSPACE}/container"){
                    sh "pwd"
                    sh 'docker build -t acr267.azurecr.io/gsd:$BUILD_NUMBER .'
                        }
            }
        }
        stage('push image'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'acr', passwordVariable: 'password', usernameVariable: 'username')]) {
                sh 'echo ${password} | docker login acr267.azurecr.io --username ${username} --password-stdin'
                sh 'docker push acr267.azurecr.io/gsd:$BUILD_NUMBER'
                }
            }
        }
        stage('Install Azure CLI'){
            steps{
                sh '''
                echo "Installing Azure CLI"
                cat /etc/os-release
                hostname
                hostnamectl
                sudo apt-get update
                curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
                az --version
                '''
            }
        }
        stage('deploy web app'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'asp', passwordVariable: 'password', usernameVariable: 'username')]) {
                sh 'az login --service-principal -u ${username} -p ${password} --tenant ${AZURE_TENANT_ID}'
                }
                withCredentials([usernamePassword(credentialsId: 'acr', passwordVariable: 'password', usernameVariable: 'username')]) {
                sh 'az webapp config container set --name tetris-webapp267 --resource-group jenkins267 --docker-custom-image-name acr267.azurecr.io/gsd:$BUILD_NUMBER --docker-registry-server-url https://acr267.azurecr.io --docker-registry-server-user ${username} --docker-registry-server-password ${password}'
                // sh 'az webapp config container set --name tetris-webapp267 --resource-group jenkins267 --docker-custom-image-name nginx:latest'
                // sh 'az webapp config container set --name tetris-webapp267 --resource-group jenkins267 --docker-custom-image-name mfk267/catcontainer:latest'
                // sh 'az webapp config container set --name tetris-webapp267 --resource-group jenkins267 --docker-custom-image-name mfk267/gsd:latest'
                }
            }
        }
    }
}