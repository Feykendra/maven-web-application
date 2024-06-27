pipeline{
  agent any 
  tools {
    maven "Maven3.9.6"
  }  
  stages {
    stage('1GetCode'){
      steps{
        sh "echo 'cloning the latest application version' "
        git 'https://github.com/Feykendra/maven-web-application'
      }
    }
    stage('2Test+Build'){
      steps{
        sh "echo 'running JUnit-test-cases' "
        sh "echo 'testing must passed to create artifacts ' "
        sh "mvn clean package"
      }
    }
    stage('3CodeQuality'){
      steps{
        sh "echo 'Perfoming CodeQualityAnalysis' "
        sh "mvn sonar:sonar"
      }
    }
    stage('4uploadNexus'){
      steps{
	sh "echo 'Upload artifacts to Nexus' "
        sh "mvn deploy"
      }
    } 
    stage('5deploy2prod'){
      steps{ 
      deploy adapters: [tomcat9(credentialsId: 'tomcatcredentials', path: '', url: 'http://172.31.29.234:8177/')], contextPath: null, onFailure: false, war: 'target/*war'
      }
    }
}
  post{
    always{
      emailext body: '''Hey guys
Please check build status.
 
Thanks
TeamCameroon
+237650661631''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'paypal-team@gmail.com'
    }
    success{
      emailext body: '''Hey guys
Good job build and deployment is successful.
 
Thanks
Landmark 
+237650661631''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'paypal-team@gmail.com'
    } 
    failure{
      emailext body: '''Hey guys
Build failed. Please resolve issues.
 
Thanks
Landmark 
+237650661631''', recipientProviders: [buildUser(), developers()], subject: 'success', to: 'paypal-team@gmail.com'
    }
  } 
}
