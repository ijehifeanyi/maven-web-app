pipeline{
  agent any
  tools{
    maven "maven3.8.5"
  }
  stages{
    stage("1.CodeClone"){
      steps{
        git credentialsId: 'GITNEW', url: 'https://github.com/ijehifeanyi/maven-web-app.git'
          //slackSend channel: 'ACADA Learning', message: 'Build Started'//
        
      }
    }
    stage("2.Build"){
      steps{
        sh "mvn clean package"
      }
    }
    stage("3.CodeQuality"){
      steps{
        sh "mvn sonar:sonar"
      }
    }
    stage("4.Artifacts"){
      steps{
        sh "mvn deploy"
      }
    }
    stage("5.DeploytoUAT"){
      steps{
        deploy adapters: [tomcat9(credentialsId: 'Tomcat-cred', path: '', url: 'http://192.168.25.15:8080/')], contextPath: null, war: 'target/*.war'
      }
    }
    stage("6.Approval"){
      steps{
        timeout(time:5, unit:'DAYS'){
        input message: 'Approval for Production'
      }
    }
  }
    stage("7.deploytoPROD"){
      steps{
        deploy adapters: [tomcat9(credentialsId: 'Tomcat-cred', path: '', url: 'http://192.168.25.15:8080/')], contextPath: null, war: 'target/*.war'
      }
    }
    stage("8.EmailNotification"){
      steps{
        emailext body: 'This is Build Success', subject: 'Build Success', to: 'ifyejay@gmail.com'
      }
    }
  }
}
post {
      success {
            slackSend color: "good", message:  " :heavy_check_mark: *SUCCESS* \n ${env.JOB_NAME} Build${env.BUILD_NUMBER}"
        }
        failure {
            slackSend color: "danger", message:  ":no_entry: *FAILED* \n ${env.JOB_NAME} Build${env.BUILD_NUMBER}"
  }
}