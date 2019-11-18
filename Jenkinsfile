pipeline {
agent any

stages {
/**Insurance-Backend Pipeline Job Build and Test stages **/
stage('SCM Checkout') {
steps {
git url:'https://github.com/DevOps-NewBranch/INGFavAccount.git'
}
}
stage('Build') {
steps {
         sh"/opt/apache-maven-3.6.2/bin/mvn clean package -Dmaven.test.skip=true"
}
}
stage("build & SonarQube analysis") {           
            steps {
              withSonarQubeEnv('Sonar_Server') {
                sh '/opt/apache-maven-3.6.2/bin/mvn sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
         }          
                
stage('Deploy') {
steps {
                     sh"/opt/apache-maven-3.6.2/bin/mvn clean deploy -Dmaven.test.skip=true"
}
}
stage('Release') {
steps {
                    sh"export JENKINS_NODE_COOKIE=dontKillMe; nohup java -jar $WORKSPACE/target/*.jar &"
}
}
}
}
