pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'development') {
                    sh 'docker build -t chloealex/duo-backend:latest -t chloealex/duo-backend:$BUILD_NUMBER .'
                    } else {
                        sh "echo 'Build not required!'"
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'development') {
                        sh '''
                        docker push chloealex/duo-backend:latest
                        docker push chloealex/duo-backend:$BUILD_NUMBER
                        '''
                    } else {
                        sh "echo 'Push not required!'"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'development') {
                        sh'''
                        kubectl apply -f . --namespace=preprod
                        kubectl rollout restart deployment backend --namespace=preprod
                        '''
                    } else if (env.GIT_BRANCH == 'main') {
                        sh'''
                        kubectl apply -f . --namespace=production
                        kubectl rollout restart deployment backend --namespace=production
                        '''
                    } else {
                        sh'''
                        echo "unrecognised branch"
                        '''
                    }
                }
            }
        }
        stage('Clean Up') { 
            steps {
                sh '''
                docker system prune -a --force
                '''
            }
        }
    }
}

