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

    stage('Descargar Odoo') {
      steps {
        ansiblePlaybook 'imagen.yml'
      }
    }

    stage('Envío Docker-compose y ejecución') {
      steps {
        sh 'ansible all -i hosts -m copy -a "src=docker-compose.yml dest=/tmp/docker-compose.yml"'
        ansiblePlaybook 'iniciar_docker.yml'
      }
    }

  }
}