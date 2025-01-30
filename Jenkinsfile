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
    stage('Building the Code with Maven'){
      agent{
        docker {image 'maven:3.8.1-adoptopenjdk-11'}
      }
      steps{
        sh 'mvn --version'
      }
    }
  }
}
