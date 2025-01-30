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
        sh 'cd java-maven-sonar-argocd-helm-k8s/spring-boot-app/'
        if (fileExists('pom.xml')) {
          echo "pom.xml found! Running Maven build..."
          sh 'mvn clean package'
          }
        else {
          echo "pom.xml not found. Skipping build."
          }
        }
      }
  }
}
}
