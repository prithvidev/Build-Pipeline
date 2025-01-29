pipeline{
  agent any
  stages{
    stage('Checking if sheel commands are working or not'){
      steps{
        sh '''
        date
        pwd
        echo "Hello"
        sh '''
      }
    }
  }
}
