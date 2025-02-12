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

    stage('Checkout the code'){
      agent {
        docker { image 'kapil0123/git' } 
      }
      steps{
        sh '''
        git clone https://github.com/prithvidev/demo.git
        sh '''
      }
    }
    
    stage('Building code with Maven'){
      agent{
        docker { image 'prithvidev/custom-maven-jdk21:v3.0'
                args '--user root -v /tmp/.m2:/root/.m2'}
      }
      steps{
        sh 'mvn --version'
        sh 'java --version'
        sh 'date'
      
        dir('demo'){
          sh 'ls -lrt'
          sh '[ -f "pom.xml" ] && mvn clean package || echo "pom.xml not found!"'
        }
      }
    }
    
    stage('SonarQube Analysis') {
            agent {
              docker { image 'prithvidev/custom-maven-jdk21:v3.0'
                args '--user root -v /tmp/.m2:/root/.m2'}
                }
            steps {
                sh '''
                mvn -f demo/pom.xml verify package sonar:sonar \
                -Dsonar.host.url=https://sonarcloud.io/ \
                -Dsonar.organization=prithvidev \
                -Dsonar.projectKey=prithvidev_prithvi-dev \
                -Dsonar.login=$SONAR_TOKEN
                '''
                }
            }
    stage('Preparing docker image for tomcat'){
      agent any
      steps{
        dir('demo'){
          //Copying the war file into working directory
            sh 'cp target/*.war .'

          //Building Docker image for running tomcat container
            script{
                def warname = sh(script: "ls *.war", returnStdout: true).trim()
                echo "WAR File Name: ${warname}"
                sh "docker build --build-arg warname=${warname} -t demo -f dockerfile ."
            }
        }
      }
    }

    stage('Running an instance of DEMO App'){
        agent any
        steps{
            script{
               dir('demo') {
                 //Stopping last running container for application
                 script{
                   def container = "tomcat"
                   sh "docker stop ${container}"
                 }
                 //Starting new Container
                 sh "docker run -d --rm --name ${container} -p 8081:8080 demo"
               }
              //Wait for 30 seconds before health check
                sh '''
                sleep 30
                response=$(curl --connect-timeout 2 --max-time 2 -o /dev/null -s -w "%{http_code}" http://54.89.85.71:8081)
                if [ "$response" -eq 200 ]; then
                    echo "✅ Application is up and running (HTTP 200)"
                else
                    echo "❌ Application failed with status: ${response}"
                exit 1
                fi
                sh '''
            }
        }
    }
  }

  post {
    always {
        script {
            def sonarUrl = "https://sonarcloud.io/project/overview?id=prithvidev_prithvi-dev"
            def consoleUrl = "${env.BUILD_URL}"
            // Get the user who triggered the job
            def user = currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause)?.getUserName() ?: "Automated Trigger"
            emailext subject: "Pipeline Status: ${currentBuild.result}",
                     body: """\
                        <html>
                        <body style="font-family:Arial, sans-serif;">
                            <h2 style="color:#2E86C1;">Jenkins Pipeline Status: <span style="color:${currentBuild.result == 'SUCCESS' ? '#28A745' : '#DC3545'}">${currentBuild.result}</span></h2>
                            <p><strong>Build Number:</strong> ${currentBuild.number}</p>
                            <hr>
                            <h3 style="color:#17A2B8;">SonarQube Analysis Successful ✅</h3>
                            <p><strong>Project:</strong> <b>DEMO</b></p>
                            <p><strong>SonarQube Report:</strong> <a href="${sonarUrl}" style="color:#007BFF; text-decoration:none;">View Report</a></p>
                            <p><strong>Jenkins Console Output:</strong> <a href="${consoleUrl}" style="color:#007BFF; text-decoration:none;">View Logs</a></p>
                            <hr>
                            <p><strong>Triggered By:</strong> ${user}</p>
                            <p style="color:#555;">Best Regards,<br><strong>Jenkins Automation</strong></p>
                        </body>
                        </html>
                        """,
                     to: 'prithvidevkanojia1@gmail.com',
                     from: 'jenkins@example.com',
                     replyTo: 'jenkins@example.com',
                     mimeType: 'text/html'
        }
    }
  }
}
