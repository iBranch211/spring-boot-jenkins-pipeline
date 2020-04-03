node {

    // docker private repositoynin ip adresi (nexus)

    def dockerImage;

    def docker_image_repository = "suayb/app"
    def docker_image_tag = "2"
    def k8s_namespace = "app"
    def k8s_component = "backend"
    def k8s_pod_name = sh(script: "kubectl get pod -n ${namespace} -l component=${component} -o jsonpath='{.items[0].metadata.name}'", returnStdout: true)

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

    stage('Build Docker Image') {
      // docker image build etme
      dockerImage = docker.build("${docker_image_repository}:${docker_image_tag}")
    }

    stage('Push Docker Image') {
      docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
        dockerImage.push("${docker_image_tag}")
      }
    }

    stage('K8s Pod Update'){
        echo "Pod Name : ${pod_name}"
        sh "kubectl delete pod ${pod_name} -n ${namespace}"
    }
}