pipeline {
    agent {
        docker {
            image 'node:9-alpine'
            args '-p 3000:3000 -p 5000:5000 -u root'
        }
    }
    // declara variáveis de ambiente.
    environment {
        // Indica pro node que ele está num ambiente de CI e que todos os
        // passos que ele fizer precisam ser feitos de forma autônoma.
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development') {
            // diretiva condicional
            when {
                // Somente executa esses passos se for o branch
                // desenvolvimento.
                branch 'develop'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'prod'
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
