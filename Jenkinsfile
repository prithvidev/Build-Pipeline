pipeline{
  agent none

  environment{
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages{
    
    stage('Clean Workspace') {
            steps {
                deleteDir()  // Deletes all files in the workspace
            }
        }

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
                sh '''
                mvn -f Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s/spring-boot-app/pom.xml verify package sonar:sonar \
                -Dsonar.host.url=https://sonarcloud.io/ \
                -Dsonar.organization=prithvidev \
                -Dsonar.projectKey=prithvidev_prithvi-dev \
                -Dsonar.login=$SONAR_TOKEN
                '''
                }
            }
    }
}
