pipeline {
    agent any

    environment {
        REPO = ''
    }

    stages {
        stage('Build repo') {
            steps {
                script {
                    echo 'Building..'
                    sh """sudo docker build . -t ${REPO}:${env.BUILD_ID}"""
                    sh """sudo docker tag ${REPO}:${env.BUILD_ID} localhost:5000/${REPO}:${env.BUILD_ID}"""
                    sh """sudo docker tag ${REPO}:${env.BUILD_ID} localhost:5000/${REPO}:latest"""
                    sh """sudo docker push localhost:5000/${REPO}:${env.BUILD_ID}"""
                    sh """sudo docker push localhost:5000/${REPO}:latest"""
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            sh "sudo ansible-playbook deploy/deploy.yml --user=jenkins --extra-vars ImageName=localhost:5000/${REPO} --extra-vars imageTag=${env.BUILD_ID} --extra-vars Name=${REPO}"
        }
        catch (err) {
            currentBuild.result = 'FAILURE'
        }
    }
}