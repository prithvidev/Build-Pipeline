pipeline{
  agent none

  environment{
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages{
    stage('Clean Workspace') {
        agent any
            steps {
                sh 'sudo rm -rf /var/lib/jenkins/workspace/Buildpipeline/*'   // Deletes all files in the workspace
            }
        }

    stage('ChecKout the code'){
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
        docker { image 'prithvidev/custom-maven-jdk21:v3.0'
                args '--user root -v /tmp/.m2:/root/.m2'}
      }
      steps{
        sh 'mvn --version'
        sh 'java --version'
        sh 'date'
      
        // dir('demo'){
        //   sh 'ls -lrt'
        //   sh '[ -f "pom.xml" ] && mvn clean package || echo "pom.xml not found!"'
        // }
      }
    }
    
    // stage('SonarQube Analysis') {
    //         agent {
    //           docker { image 'prithvidev/custom-maven-jdk21:v3.0'
    //             args '--user root -v /tmp/.m2:/root/.m2'}
    //             }
    //         steps {
    //             sh '''
    //             mvn -f demo/pom.xml verify package sonar:sonar \
    //             -Dsonar.host.url=https://sonarcloud.io/ \
    //             -Dsonar.organization=prithvidev \
    //             -Dsonar.projectKey=prithvidev_prithvi-dev \
    //             -Dsonar.login=$SONAR_TOKEN
    //             '''
    //             }
    //         }
  }

  post {
    always {
            script {
                def sonarUrl = "https://sonarcloud.io/project/overview?id=prithvidev_prithvi-dev"
                emailext subject: "Pipeline Status : ${currentBuild.result}",
                         body: """<p>BUILD Status : ${currentBuild.result}</p>
                                  <p>Build Number : ${currentBuild.number}</p>
                                  <h3>SonarQube Analysis Successful</h3>
                                  <p>Project: <b>DEMO</b></p>
                                  <p><a href="${sonarUrl}">View Report</a></p>
                                  <p>Check the <a href="${env.BUILD_URL}"> Console Output</a>.</p>""",
                         to: 'prithvidevkanojia1@gmail.com',
                         from: 'jenkins@example.com'
                         replyTo: 'jenkins@example.com'
                         mimeType: 'text/html'
            }
        }
  }
}
