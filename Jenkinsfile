node {
    def dockerImage;

    def docker_image_repository = "suayb/app"
    def docker_image_tag = "2"
    def docker_registry_url = "https://registry.hub.docker.com"
    def k8s_namespace = "app"
    def k8s_component = "backend"
    def k8s_pod_name = sh(script: "kubectl get pod -n ${k8s_namespace} -l component=${k8s_component} -o jsonpath='{.items[0].metadata.name}'", returnStdout: true)

   stage("Checkout Scm") {
     checkout scm
   }

    def mvnContainer = docker.image("maven:3-alpine")
     mvnContainer.inside("-v $HOME/.m2:/root/.m2 -v $PWD:/app -w /app") {
         stage("Build Project") {
              //maven ile proje build etme
              sh "mvn clean install"
           }
    }

    stage("Build Docker Image") {
      dockerImage = docker.build("${docker_image_repository}:${docker_image_tag}")
    }

    /* stage("Push Docker Image") {
      docker.withRegistry("${docker_registry_url}", "dockerhub") {
        dockerImage.push("${docker_image_tag}")
      }
    }

    stage("K8s Pod Update"){
        echo "Pod Name : ${k8s_pod_name}"
        sh "kubectl delete pod ${k8s_pod_name} -n ${k8s_namespace}"
    } */


     stage("Helm Blue Slot Open"){
         dir('charts/app'){
            sh "helm upgrade ${k8s_namespace} . --namespace ${k8s_namespace} --set ${k8s_component}.blue.enabled=true --reuse-values"
          }
     }

     stage("Helm Blue Slot Close"){
          dir('charts/app'){
            sh "helm upgrade ${k8s_namespace} . --namespace ${k8s_namespace} --set ${k8s_component}.blue.enabled=false --reuse-values"
          }
      }
}