pipeline {

  agent any

  stages {
// Github integration
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/vamsijakkula/hellowhale.git', branch:'master'
      }
    }
 
 //Build Docker Image 
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("vamsijakkula/hellowhale:${env.BUILD_ID}")
                }
            }
        }
// Push the Docker image to Docker repository 
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
//Deploy the Application via Kubernetes
    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
