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
      steps {
        ansiblePlaybook 'imagen.yml'
      }
    }

    stage('Enviar compose e iniciar contenedor') {
      steps {
        sh 'ansible worker1 -i hosts -m copy -a "src=docker-compose.yml dest=/tmp/docker-compose.yml"'
        ansiblePlaybook 'iniciar-docker.yml'
        sh 'ansible worker2 -i hosts -m copy -a "src=docker-compose2.yml dest=/tmp/docker-compose2.yml"'
        ansiblePlaybook 'iniciar_docker2.yml'
      }
    }

  }
}