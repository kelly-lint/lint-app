node('maven-label') {
   def mvnHome
   stage('Preparation') { 
      git branch: '${BRANCH_NAME}', url: 'https://github.com/kelly-lint/lint-app.git'         
      mvnHome = tool 'maven-3.6.3'
   }
   stage('Build') {
     
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean install sonar:sonar -Dsonar.host.url=http://ec2-3-14-148-86.us-east-2.compute.amazonaws.com:9000/'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
   }
}
