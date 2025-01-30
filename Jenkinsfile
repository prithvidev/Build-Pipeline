pipeline{
  agent none
  stages{
    stage('Checout the code'){
      agent {
        docker { image 'kapil0123/git' } 
      }
      steps{
        sh '''
        git clone https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero.git
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
        dir('Jenkins-Zero-To-Hero'){
          sh 'ls -lrt'
          sh 'cd java-maven-sonar-argocd-helm-k8s/spring-boot-app'
          sh 'ls -lrt'
          sh 'mvn clean package'
        }
      }
  }
}
}
