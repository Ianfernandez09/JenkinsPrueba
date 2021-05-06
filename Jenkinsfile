pipeline {
  agent any
  stages {
    stage('Ping') {
      steps {
        sh 'ansible all -i hosts -m ping -f 5'
      }
    }

    stage('Instalar Docker') {
      steps {
        ansiblePlaybook 'dependencias.yml'
        sh 'ansible all -i hosts -a "sudo systemctl enable docker"'
      }
    }

    stage('Descargar imagen Odoo') {
      parallel {
        stage('Descargar imagen Odoo') {
          steps {
            ansiblePlaybook 'imagen.yml'
          }
        }

        stage('Descargar imagen apache2') {
          steps {
            ansiblePlaybook 'imagen2.yml'
          }
        }

      }
    }

    stage('Enviar compose e iniciar contenedor') {
      steps {
        sh 'ansible all -i hosts -m copy -a "src=docker-compose.yml dest=/tmp/docker-compose.yml"'
        ansiblePlaybook 'iniciar-docker.yml'
      }
    }

  }
}