pipeline {
  agent any
  stages {
    stage('Lint HTML') {
      steps {
        sh 'tidy -q -e *.html'
      }
    }

    stage('Build Docker Image') {
      steps {
        withCredentials(bindings: [[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
          sh '''
						docker build -t ozturq/capstone .
					'''
        }

      }
    }

    stage('Push Image To Dockerhub') {
      steps {
        withCredentials(bindings: [[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
          sh '''
						docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
						docker push ozturq/capstone
					'''
        }

      }
    }

    stage('Set current kubectl context') {
      steps {
        withAWS(region: 'us-east-1', credentials: 'aws_cred') {
          sh '''
						kubectl config use-context arn:aws:eks:us-east-1:614654026197:cluster/ozturqcapstonecls
					'''
        }

      }
    }

    stage('Deploy blue') {
      steps {
        withAWS(region: 'us-east-1', credentials: 'aws_cred') {
          sh '''
						kubectl apply -f ./blue-controller.json
					'''
        }

      }
    }

    stage('Deploy green') {
      steps {
        withAWS(region: 'us-east-1', credentials: 'aws_cred') {
          sh '''
						kubectl apply -f ./green-controller.json
					'''
        }

      }
    }

    stage('to blue') {
      steps {
        withAWS(region: 'us-east-1', credentials: 'aws_cred') {
          sh '''
						kubectl apply -f ./blue-service.json
					'''
        }

      }
    }

    stage('approvement') {
      steps {
        input 'Ready to redirect traffic to green?'
      }
    }

    stage('to green') {
      steps {
        withAWS(region: 'us-east-1', credentials: 'aws_cred') {
          sh '''
						kubectl apply -f ./green-service.json
					'''
        }

      }
    }

  }
}