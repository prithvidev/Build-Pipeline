pipeline{
  agent none
  stages{
    stage('Checout the code'){
      agent {
        docker { image 'alpine/git' } 
      }
      steps{
        sh '''
        git -version
        sh '''
      }
    }
  }
}
