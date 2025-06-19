pipeline {
  agent { 
    label 'agent-vm'
  }

  environment {
    // Chemin de destination sur la machine de l'agent
    DESTINATION_PATH = '/var/www/html/'
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
    stage('Copie index.html'){
      steps{
        script {
          // Copie le fichier index.html vers la destination
          sh "sudo cp index.html ${DESTINATION_PATH}"
          echo "Le fichier index.html a été copié vers ${DESTINATION_PATH}"
        }
      }
    }
    stage('Restart Apache2') {
      steps {
        script {
          echo "Redémarrage du service Apache2..."
          sh "sudo systemctl restart apache2"
        }
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
