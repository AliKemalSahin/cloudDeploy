pipeline 
{
    agent any
    environment {
        DOCKER_IMAGE_NAME = "alikemal/deploycloud"
    }
    tools 
    {
        maven 'M3'
    }
    stages 
    {
        stage('Build Jar')
        {
            steps 
            {
                sh '''
                 mvn clean package
                 cd target
                 cp rest-service-0.0.1-SNAPSHOT.jar rest-service.jar 
                '''
                stash includes: 'target/*.jar', name: 'targetfiles'
            }
        }
        stage('Build Docker Image') 
        {
            steps 
            {
                script 
                {          
                    sh 'gcloud builds submit --tag gcr.io/future-pulsar-292310/clouddeploy' 
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        /*
         stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                
            }
        }   */
        
    }
    
}

