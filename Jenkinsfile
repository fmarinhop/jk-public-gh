pipeline {
    agent any
    
    stages {
        stage('Cloning Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/fmarinhop/jk-public-gh.git'
            }
        }
        stage('Building Images') {
            steps {
                sh 'docker build -t webapp:${BUILD_NUMBER} .'
            }
        }
        stage('Deploying Application') {
            steps {
                script {
                    // Verifica se o contêiner existe antes de tentar pará-lo
                    def containerExists = sh(script: 'docker ps -a --format "{{.Names}}" | grep -w webapp_ctr || true', returnStdout: true).trim()
                    
                    if (containerExists) {
                        echo "Container webapp_ctr encontrado. Parando..."
                        sh 'docker stop webapp_ctr || true'
                    } else {
                        echo "Container webapp_ctr não existe. Pulando o comando de parada."
                    }
                    
                    // Inicia o novo contêiner
                    sh 'docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}'
                }
            }
        }
    }
}
