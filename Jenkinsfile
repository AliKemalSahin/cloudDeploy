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
                 
                    sh 'curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-17.04.0-ce.tgz \

  && tar xzvf docker-17.04.0-ce.tgz \
  && chmod u+x+w+r /usr/local/bin/docker \
  && mv docker/docker /usr/local/bin \
  && rm -r docker docker-17.04.0-ce.tgz'
                    app = docker.build(DOCKER_IMAGE_NAME) 
                   
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

