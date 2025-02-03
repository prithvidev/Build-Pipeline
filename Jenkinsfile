pipeline{
  agent none

  environment{
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages{
    
    stage('Clean Workspace') {
        agent any
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
        git clone https://github.com/prithvidev/demo.git
        sh '''
      }
    }
    
    stage('Maven'){
      agent{
        docker { image 'maven:3.8.7-openjdk-18-slim' }
      }
      steps{
        sh 'mvn --version'
        sh 'java --version'
        sh 'date'
      
        dir('demo'){
          sh 'ls -lrt'
          sh '[ -f "pom.xml" ] && mvn clean package || echo "pom.xml not found!"'
        }
        sh '''
                
                mvn -f demo/pom.xml verify package sonar:sonar \
                -Dsonar.host.url=https://sonarcloud.io/ \
                -Dsonar.organization=prithvidev \
                -Dsonar.projectKey=prithvidev_prithvi-dev \
                -Dsonar.login=$SONAR_TOKEN
                '''
      }
    }
    
    // stage('SonarQube Analysis') {
    //         agent {
    //           docker { image 'sonarsource/sonar-scanner-cli' }
    //         }
    //         steps {
    //             sh '''
    //             mvn -f Jenkins-Zero-To-Hero/java-maven-sonar-argocd-helm-k8s/spring-boot-app/pom.xml verify package sonar:sonar \
    //             -Dsonar.host.url=https://sonarcloud.io/ \
    //             -Dsonar.organization=prithvidev \
    //             -Dsonar.projectKey=prithvidev_prithvi-dev \
    //             -Dsonar.login=$SONAR_TOKEN
    //             '''
    //             }
    //         }
    }
}
