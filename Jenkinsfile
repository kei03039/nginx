pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/kei03039/nginx.git', branch: 'main'
      }
    }
    
    stage('docker build') {
      steps {
        sh '''
        docker build -t multi-img .
        docker tag multi-img 192.168.0.51:5000/nginx
        docker push 192.168.0.51:5000/nginx
        '''
      }
    }
    
    stage('deploy kubernetes') {
      steps {
        sh '''
        kubectl create deployment nginx-2 --image=192.168.0.51:5000/multi-img
        kubectl expose deployment nginx-2 --type=LoadBalancer --port=8080 \
                                               --target-port=80 --name=nginx-svc-2
        '''
      }
    }
  }
}
