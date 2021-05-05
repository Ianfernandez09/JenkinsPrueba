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

  }
}