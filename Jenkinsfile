pipeline{
  agent none
  stages{
    stage('Checout the code'){
      agent {
        docker { image 'kapil0123/git' } 
      }
      steps{
        sh '''
        git clone https://github.com/Kapil987/test_maven.git
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
        dir('test_maven'){
          sh '''
          cd demo/demo
          mvn clean install
          sh '''
        }
      }
    }
  }
}
