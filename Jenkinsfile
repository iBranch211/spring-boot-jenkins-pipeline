node {
    // maven referansı
    // ** NOT: Bu 'maven' Maven aracı Jenkins Global Yapılandırması'nda yapılandırılmalıdır.
    def mvnHome = tool 'maven'

    // docker imagesine referans tutar
    def dockerImage
    // docker private repositoynin ip adresi (nexus)

    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"

    stage('Clone Repo') { // görüntüleme amacıyla
      // GitHub repositoryden bazı kodlar alın
      git 'https://github.com/felipemeriga/DevOps-Example.git'
      // Maven toolunu alın.
      // ** NOT: Bu 'maven-3.5.2' Maven aracı yapılandırılmalıdır
      // ** global yapılandırmada.
      mvnHome = tool 'maven'
    }

    stage('Build Project') {
      //maven ile proje build etme
      sh "'${mvnHome}/bin/mvn' clean install"
    }

    stage('Build Docker Image') {
      // docker image build etme
      dockerImage = docker.build("devopsexample:${env.BUILD_NUMBER}")
    }

    stage('Deploy Docker Image'){

      // docker imagesini nexus'a dağıtma

      echo "Docker Image Tag Name: ${dockerImageTag}"

	  sh "docker stop devopsexample"

	  sh "docker rm devopsexample"

	  sh "docker run --name devopsexample -d -p 2222:2222 devopsexample:${env.BUILD_NUMBER}"

	  // docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
      //    dockerImage.push("${env.BUILD_NUMBER}")
      //      dockerImage.push("latest")
      //  }

    }
}