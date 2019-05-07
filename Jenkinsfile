node {
  try {
    stage('Checkout') {
      checkout scm
    }
    stage('Environment') {
      sh 'git --version'
      echo "Branch: ${env.BRANCH_NAME}"
      sh 'docker -v'
      sh 'printenv'
    }
    stage('Build Docker test'){
     sh 'docker build -t react-test -f Dockerfile.test --no-cache .'
    }
    stage('Docker test'){
      sh 'docker run --rm react-test'
    }
    stage('Clean Docker test'){
      sh 'docker rmi react-test'
    }
    stage('Deploy'){
      if(env.BRANCH_NAME == 'master'){
        sh 'docker build -t docker-create-react-app --no-cache .'
        sh 'docker tag docker-create-react-app localhost:5000/docker-create-react-app'
        sh 'docker push localhost:5000/docker-create-react-app'
        sh 'docker rmi -f docker-create-react-app localhost:5000/docker-create-react-app'
      }
    }
  }
  catch (err) {
    throw err
  }
}
