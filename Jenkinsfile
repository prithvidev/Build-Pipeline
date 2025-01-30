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
        dir('Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s/spring-boot-app'){
          sh 'ls -lrt'
          sh '[ -f "pom.xml" ] && mvn clean package || echo "pom.xml not found!"'
        }
      }
    }
    
    stage('SonarQube Analysis') {
            agent {
              docker { image 'sonarsource/sonar-scanner-cli' }
            }
            steps {
                dir('Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s/spring-boot-app/target') {
                    sh '''
                    sonar-scanner \
                        -Dsonar.projectKey=your-project \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONARQUBE_URL} \
                        -Dsonar.login=${SONARQUBE_TOKEN}
                    '''
                }
            }
      }
}
}
