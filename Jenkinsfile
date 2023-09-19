pipeline {
    agent { label 'docker-build-node' }
    environment {
              APP_NAME = 'register-app-pipeline' 
    }
    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }
        stage("checkout from scm") {
            steps {
                git branch: 'main', credentialsId: 'git', url: 'https://github.com/Prabhuk8/Jenkins-cd-registry-app-k8s'
            }
        }
        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "prabhuk8"
                   git config --global user.email "prabhu.pattilingam@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'git', gitToolName: 'Default')]) {
                  sh "git push https://github.com/Prabhuk8/Jenkins-cd-registry-app-k8s main"
                }
           }
       }
    }
}
