pipeline {
    agent any
    environment{
        DOCKER_HUB_LOGIN = credentials('docker')
        REGISTRY = "roxsross12"
        REPOSITORY= "retail-store-app"
        SERVER= "ec2-user@35.84.193.83"
    }
    stages {
        stage('docker build') {
            steps {
               sh 'cd src/assets && docker build -t $REGISTRY/$REPOSITORY:assets-v1 .'
               sh 'cd src/catalog && docker build -t $REGISTRY/$REPOSITORY:catalog-v1 .'
               sh 'cd src/checkout && docker build -t $REGISTRY/$REPOSITORY:checkout-v1 .'
               sh 'cd src/orders && docker build -t $REGISTRY/$REPOSITORY:orders-v1 .'
               sh 'cd src/ui && docker build -t $REGISTRY/$REPOSITORY:ui-v1 .'
               sh 'cd src/cart && docker build -t $REGISTRY/$REPOSITORY:cart-v1 .'
            }
        }
        stage('docker push') {
            steps {
                sh '''
                docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW
                docker push $REGISTRY/$REPOSITORY:assets-v1
                docker push $REGISTRY/$REPOSITORY:catalog-v1
                docker push $REGISTRY/$REPOSITORY:checkout-v1
                docker push $REGISTRY/$REPOSITORY:orders-v1
                docker push $REGISTRY/$REPOSITORY:ui-v1
                docker push $REGISTRY/$REPOSITORY:cart-v1
                '''
            }
        }
        stage('update compose') {
            steps {
                sh '''
                sed -i -- "s/REGISTRY/$REGISTRY/g" docker-compose.prod.yml
                sed -i -- "s/REPOSITORY/$REPOSITORY/g" docker-compose.prod.yml
                cat docker-compose.prod.yml
                '''
            }
        } 
        stage('Deploy To EC2') {
            steps {
                sshagent (['ssh-aws']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no docker-compose.prod.yml $SERVER:/home/ec2-user
                    ssh -o StrictHostKeyChecking=no $SERVER mv docker-compose.prod.yml docker-compose.yml
                    ssh -o StrictHostKeyChecking=no $SERVER docker-compose up -d 
                    '''

                }
            }
        }                
    }
}