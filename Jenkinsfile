node {
    // maven referansı
    // ** NOT: Bu 'maven' Maven aracı Jenkins Global Yapılandırması'nda yapılandırılmalıdır.
    def mvnHome = tool 'maven'

    // docker imagesine referans tutar
    def dockerImage
    // docker private repositoynin ip adresi (nexus)

    def dockerImageTag = "suayb/app"

   stage('checkout') {
           checkout scm
   }

    stage('Build Project') {
      //maven ile proje build etme
      sh "'${mvnHome}/bin/mvn' clean install"
    }

    stage('Build Docker Image') {
      // docker image build etme
      dockerImage = docker.build("${dockerImageTag}:${env.BUILD_NUMBER}")
    }

    stage('Deploy Docker Image'){

      // docker imagesini nexus'a dağıtma

      echo "Docker Image Tag Name: ${dockerImageTag}"

      sh "docker-compose up -d"

	 // sh "docker stop devopsexample"

	 // sh "docker rm devopsexample"

	 // sh "docker run --name devopsexample -d -p 2222:2222 devopsexample:${env.BUILD_NUMBER}"

	 //  docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
     //     dockerImage.push("${env.BUILD_NUMBER}")
          // dockerImage.push("latest")
      //  }

    }
}