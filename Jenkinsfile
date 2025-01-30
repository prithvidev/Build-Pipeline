pipeline{
  agent none
  stages{
    stage('Checout the code'){
      agent {
        docker { image 'alpine/git' } 
      }
      steps{
        sh '''
        git clone https://github.com/Kapil987/test_maven.git
        sh '''
      }
    }
  }
}
