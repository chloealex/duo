pipeline{

        agent any

        stages{

            stage('Build docker images on Jenkins'){

                steps{

                    sh '''

                    docker build -t chloealex/duo-nginx:latest ./nginx

                    docker build -t chloealex/duo-flask:latest .

                    '''

                }

            }

            stage('Push images to Docker Hub'){

                steps{

                    sh '''

                    docker push chloealex/duo-nginx:latest

                    docker push chloealex/duo-flask:latest

                    '''

                }

            }

            stage('Run a rollout restart'){

                steps{

                    sh '''

                    kubectl apply -f .

                    kubectl rollout restart deployment flask-deployment

                    '''

                }

            }

        }

}

