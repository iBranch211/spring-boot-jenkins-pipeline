node {
    // docker imagesine referans tutar
    def dockerImage
    // docker private repositoynin ip adresi (nexus)

    def dockerImageTag = "suayb/app"
    def namespace = "app"
    def component = "backend"

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

      sh "kubectl get po -n ${namespace} -l component=${component} -o jsonpath="{.items[0].metadata.name}""
      //sh 'kubectl -n ${namespace} get pod -l component=${component} -o jsonpath="{.items[0].metadata.name}"'

      // echo "Pod Name : ${POD}"


     /*  docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
          dockerImage.push("${env.BUILD_NUMBER}")
                // dockerImage.push("latest")
      } */

     /*  dir('charts/app'){
          sh "helm upgrade app . --set backend.image.tag=${env.BUILD_NUMBER}"
      } */
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