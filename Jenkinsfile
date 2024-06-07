pipeline{
    agent{
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "pipeline-demoapplicatie"
    
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
    
        
        stage("Checkout from SCM"){
            steps {
                git branch:'main', credentialsId: 'github-pat', url: 'https://github.com/David1175/Appdeployment'
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
                   git config --global user.name "David1175"
                   git config --global user.email "d.espirito@student.fontys.nl"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github-pat', gitToolName: 'Default')]) {
                  sh "git push https://github.com/David1175/Appdeployment main"
                }
            }
        }
    
    }

}        