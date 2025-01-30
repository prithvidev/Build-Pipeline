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
        git clone https://github.com/Kapil987/test_maven.git
        ls -lrt
        sh '''
      }
    }
    stage('Maven'){
      agent{
        docker { image 'maven:3.8.5-openjdk-11' }
      }
      steps{
        sh 'mvn --version'
        sh 'date'
      }
    }
  }
}
