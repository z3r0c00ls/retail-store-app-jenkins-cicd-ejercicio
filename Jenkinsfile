pipeline {
    agent any
    environment{
        DOCKER_HUB_LOGIN = credentials('docker')
        REGISTRY = "roxsross12"
        REPOSITORY= "retail-store-app"
    }
    stages {
        stage('docker build') {
            steps {
               sh 'cd src/assets && docker build -t $REGISTRY/$REPOSITORY:assets-v1 .'
               sh 'cd src/catalog && docker build -t $REGISTRY/$REPOSITORY:catalog-v1 .'
               sh 'cd src/checkout && docker build -t $REGISTRY/$REPOSITORY:checkout-v1 .'
            }
        }
        stage('Deploy to hub') {
            steps {
                sh '''
                docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW
                docker push $REGISTRY/$REPOSITORY:assets-v1
                docker push $REGISTRY/$REPOSITORY:catalog-v1
                docker push $REGISTRY/$REPOSITORY:checkout-v1
                '''
            }
        }
    }
}