pipeline {
  agent { 
    label 'agent-vm'
  }

  environment {
    // Chemin de destination sur la machine de l'agent
    DESTINATION_PATH = '/var/www/html/'
  }

  stages {
    stage('Checkout') {
      steps {
        // Récupère le contenu du dépôt Git
        checkout scm
        
        // Affiche les fichiers récupérés pour vérification
        sh 'ls -la'
        
        // Vérifie que le fichier index.html existe bien
        sh 'test -f index.html && echo "index.html trouvé" || echo "ATTENTION: index.html non trouvé"'
      }
    }
    stage('Copie index.html'){
      steps{
        script {
          // Copie le fichier index.html vers la destination
          sudo sh "cp index.html ${DESTINATION_PATH}"
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
}
