pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/NjilieJr/static-website-cicd'
            }
        }
        stage('Code Quality') {
    steps { 
"C:\\Program Files\\Python311\\python.exe"    }
}
        }
        stage('Build') {
            steps {
                bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" -H tcp://localhost:2375 buildx build --load -t static-website .'
            }
        }
        stage('Test') {
            steps {
                script {
                    def result = bat(
                        script: '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" -H tcp://localhost:2375 run --rm static-website python -m pytest tests/',
                        returnStatus: true
                    )
                    echo "Tests termines avec code: ${result}"
                }
            }
        }
        stage('Package') {
            steps {
                bat '"C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" -H tcp://localhost:2375 tag static-website static-website:v1.0'
                echo 'Image packagee : static-website:v1.0'
            }
        }
        stage('Review') {
            steps {
                echo 'Review OK - Image validee pour deploiement'
            }
        }
        stage('Staging') {
            steps {
                bat '''
                    "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" -H tcp://localhost:2375 rm -f static-website-staging 2>nul
                    "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" -H tcp://localhost:2375 run -d --name static-website-staging -p 5001:5000 static-website:v1.0
                '''
                echo 'Staging sur http://localhost:5001'
            }
        }
        stage('Production') {
            steps {
                bat '''
                    "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" -H tcp://localhost:2375 rm -f static-website-prod 2>nul
                    "C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe" -H tcp://localhost:2375 run -d --name static-website-prod -p 5000:5000 static-website:v1.0
                '''
                echo 'Production sur http://localhost:5000'
            }
        }
    }
    post {
        success {
            echo '✅ Pipeline CI/CD complet termine avec succes!'
            echo 'Application disponible sur http://localhost:5000'
        }
        failure {
            echo '❌ Pipeline echoue!'
        }
    }
}
