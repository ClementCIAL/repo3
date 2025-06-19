pipeline {
  agent { 
    label 'agent-vm' 
  }

  environment {
    NOM_IMAGE = 'mon-image'
    NOM_CONTENEUR = 'mon-conteneur'
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
          sh "cp index.html ${DESTINATION_PATH}"
          echo "Le fichier index.html a été copié vers ${DESTINATION_PATH}"
      }
    }
    stage('Restart Apache2') {
      steps {
        script {
          echo "Redémarrage du service Apache2..."
          sh "sudo systemctl restart apache2"
          
          // Vérification que Apache2 s'est bien redémarré
          def apacheStatus = sh(
            script: "sudo systemctl is-active apache2",
            returnStatus: true
          )
                    
          if (apacheStatus == 0) {
            echo "Apache2 a été redémarré avec succès et est actif."
          } else {
            error "Échec du redémarrage d'Apache2. Vérifiez les journaux système."
          }
        }
      }
    }  
  }
}
