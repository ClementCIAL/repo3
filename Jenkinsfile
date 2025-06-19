pipeline {
    agent {
        label 'agent-vm'
    }
    
    stages {
        stage('Verifier et installer apache2') {
            steps {
                sh '''
                    if [ ! -f /usr/sbin/apache2 ]; then
                        sudo apt update
                        sudo apt install -y apache2
                    fi
                '''
            }
        }
        
        stage('Copier index.html') {
            steps {
                sh 'sudo cp index.html /var/www/html/index.html'
            }
        }
        
        stage('Redémarrer Apache2') {
            steps {
                sh 'sudo systemctl restart apache2'
            }
        }
    }
    
    post {
        success {
            echo 'Déploiement HTML effectué et Apache redémarré.'
        }
        failure {
            echo 'Une erreur est survenue pendant le déploiement.'
        }
    }
}
