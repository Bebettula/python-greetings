pipeline {
    agent any
    triggers{
        pollSCM('*/1 * * * *')
    }
    stages {
        stage('build') {
            steps {
                script{
                    buildup()
                }
            }
        }
        stage('deploy-dev') {
            steps {
                deploy("dev")
            }
        }
        stage('test-dev') {
            steps {
                test("dev")
            }
        }
        stage('deploy-stg') {
            steps {
                deploy("stg")
            }
        }
        stage('test-stg') {
            steps {
                test("stg")
            }
        }
        stage('deploy-prod') {
            steps {
                deploy("prod")
            }
        }
        stage('test-prod') {
            steps {
                test("prod")
            }
        }
    }
    
}

def buildup(){
    echo "Building of Python microservices is starting."
    sh "docker build -t betija/python-greetings-app:latest ."
    
    echo "Pushing image to docker registry"
    sh "docker push betija/python-greetings-app:latest"
}

def deploy(String environment){
    echo "Deploying Python microservice to ${environment} environment."

    sh "docker pull betija/python-greetings-app:latest"

    sh "docker compose stop python-greetings-app-${environment}"
    sh "docker compose rm python-greetings-app-${environment}"
    sh "docker compose up -d python-greetings-app-${environment}"
}

def test(String environment){
    echo "Python test executuon against ${environment} environment."
    sh "docker pull betija/api-tests"
    sh "docker run --network=host --rm betija/api-tests:latest run greetings greetings_${environment}"

}
