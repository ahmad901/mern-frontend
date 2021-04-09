env.DOCKER_REGISTRY = 'ahmadwizam'
env.DOCKER_IMAGE_NAME = 'mern-frontend'
node('master') {
	stage('Mern Frontend') {
      echo 'Mern Frontend'
    }
    stage('Git Clone from Github') {
        git url: 'https://github.com/ahmad901/mern-frontend.git' 
    }
    stage('Build Docker Image') {    
        sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."   
    }
    stage('Push Docker Image to Docker Hub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('Deploy To Kubernetes Cluster') {
        sh'''sed -i "20d" s-frontend.yml'''
        sh'''sed -i "19 a \'\\'        image: hegiwibowo/mern-frontend:${BUILD_NUMBER}" s-frontend.yml && sed -i "s/''//"  s-frontend.yml'''
        sh "kubectl apply -f  s-frontend.yml"
    }
    stage('Remove Docker Image in local') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"   
    }     
    stage("Clean Workspace") {
        cleanWs()
    }
}
