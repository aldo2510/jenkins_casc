
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        echo "Hola desde Jenkins con JCasC 🚀"
      }
    }
    stage('Usar secreto') {
      steps {
        withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
          sh '''
            echo "El token está disponible como variable (no lo imprimimos por seguridad)"
            echo "Longitud del token: ${#GITHUB_TOKEN}"
          '''
        }
      }
    }
  }
}
