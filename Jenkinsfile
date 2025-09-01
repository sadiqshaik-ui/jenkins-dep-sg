pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/sadiqshaik-ui/jenkins-dep-sg.git'
      }
    }

    stage('Verify Docker') {
      steps {
        sh 'docker --version'
      }
    }

    stage('Build Image') {
      steps {
        sh 'docker build -t jenkins-demo-app:latest .'
      }
    }

    stage('Deploy Container') {
      steps {
        sh '''
          # Stop & remove previous container if exists
          docker ps -q --filter "name=jenkins-demo" | grep -q . && docker stop jenkins-demo && docker rm jenkins-demo || true

          # Run new container mapping host 8081 -> container 80 (Nginx)
          docker run -d --name jenkins-demo -p 8081:80 jenkins-demo-app:latest
        '''
      }
    }
  }

  post {
    success {
      echo "✅ Deployed! Open: http://20.193.147.47:8081"
    }
    failure {
      echo "❌ Deployment failed"
    }
  }
}
