node {
    // docker imagesine referans tutar
    def dockerImage
    // docker private repositoynin ip adresi (nexus)

    def dockerImageTag = "suayb/app"

   stage('checkout') {
     checkout scm
   }

    def mvnContainer = docker.image('maven:3-alpine')
     mvnContainer.inside('-v $HOME/.m2:/root/.m2 -v $PWD:/app -w /app') {
         stage('Build Project') {
              //maven ile proje build etme
              sh "mvn clean install"
           }
    }


    stage('ls') {
        //maven ile proje build etme
        sh "ls"
    }


    stage('Build Docker Image') {
      // docker image build etme
      dockerImage = docker.build("${dockerImageTag}:${env.BUILD_NUMBER}")
    }

    stage('Deploy Docker Image'){

      // docker imagesini nexus'a dağıtma

      echo "Docker Image Tag Name: ${dockerImageTag}"

      dir('charts/app'){
          sh "ls"

          sh "helm dep up"

          sh "helm install . --name app"
      }








     // sh "docker-compose down"

    //  sh "docker-compose up -d --build"

	 // sh "docker stop devopsexample"

	 // sh "docker rm devopsexample"

	 // sh "docker run --name devopsexample -d -p 2222:2222 devopsexample:${env.BUILD_NUMBER}"

	 //  docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
     //     dockerImage.push("${env.BUILD_NUMBER}")
          // dockerImage.push("latest")
      //  }

    }
}