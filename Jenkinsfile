pipeline{
  agent none
  stages{
    stage('Checking if sheel commands are working or not'){
      agent { docker { image 'ubuntu' } }
      steps{
        sh '''
        pwd
        date
        sh '''
      }
    }
  }
}
