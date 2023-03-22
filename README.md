# maven_project1

# script : pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Deploy') {
      steps {
        withCredentials([
          usernamePassword(
            credentialsId: 'tomcat-creds',
            usernameVariable: 'TOMCAT_USERNAME',
            passwordVariable: 'TOMCAT_PASSWORD'
          )
        ]) {
          sh '''
            curl -u $TOMCAT_USERNAME:$TOMCAT_PASSWORD \
            --upload-file target/addressbook.war \
            http://localhost:8080/manager/text/deploy?path=/addressbook&update=true
          '''
        }
      }
    }
  }
}
 
 
 
# Here's how the script works:

1.The script uses the pipeline block to define a Jenkins pipeline.

2.The agent block specifies that the pipeline should run on any available Jenkins agent.

3.The stages block defines two stages: Build and Deploy.

4.The Build stage runs the build command to package the AddressBook application. The sh command runs the mvn clean package command to build and package 
the application.

5.The Deploy stage deploys the packaged WAR file on Tomcat. 

6.The withCredentials block specifies the Tomcat credentials to use for authentication. 

7.Replace the credentialsId value with the ID of your Tomcat credentials in Jenkins. 

8.The usernameVariable and passwordVariable values specify the environment variables to use for the Tomcat username and password. 

9.The sh command uses curl to upload the WAR file to Tomcat using the Tomcat manager API. 

10.Replace the Tomcat URL with your Tomcat server URL, and replace the WAR file name with the name of your packaged WAR file.


# Note: that this script assumes that you have already set up Tomcat and Jenkins with the necessary configuration, such as creating a Tomcat user with the appropriate permissions and installing the necessary plugins in Jenkins. Additionally, the script assumes that the AddressBook application is set up correctly and that the Maven build command produces a valid WAR file.





