pipeline {
environment {
DOCKER_ID = "fallewi"
DOCKER_IMAGE = "app"
DOCKER_TAG = "v1.0"
}
agent any
stages {
stage(' Docker Build'){

steps {

script {
sh '''
docker rm -f jenkins
docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG .
sleep 5
'''
}
}
}
stage(' Docker run'){

steps {

script {
sh '''
docker run -d -p 80:80 --name jenkins $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
sleep 10
'''
}
}
}

stage('Test Acceptance'){

steps {

script {
sh '''
curl localhost | grep -i "BIENVENUE SUR JENKINS EAZYTRAINING"
'''
}
}

}
stage('Docker Push'){
environment
{

DOCKER_PASS = credentials("DOCKER_HUB_PASS")

}

steps {

script {
sh '''
docker login -u $DOCKER_ID -p $DOCKER_PASS
docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
'''
}
}

}
}

}
