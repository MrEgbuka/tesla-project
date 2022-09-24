node{
    def mavenHome = tool name: 'maven3.8.6'
    stage('1CloneCode'){
    git "https://github.com/MrEgbuka/tesla-project"
    }
    stage('2Test&Build'){
    sh "${mavenHome}/bin/mvn clean package"
    }
    stage('3CodeQuality'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('4UploadArtifacts'){
    sh "${mavenHome}/bin/mvn deploy"
    }
    stage('5Deploy2UAT'){
    sh "echo 'Deploy to UAT' "
    deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://54.193.71.113:8080/')], contextPath: null, war: 'target/*war'
    }
    stage('6ApprovalGate'){
    timeout(time:5, unit:'MINUTES') {
    sh "echo 'Application ready for review' "
    input message: 'Application ready for deployment, please review and act accordingly'
  }
  }
    stage('7Deploy2Prod'){
    sh "echo 'Deploy to Production' "
    deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://54.193.71.113:8080/')], contextPath: null, war: 'target/*war'
    }
    stage('8EmailNotification'){
    emailext body: '''Hi Team,
    Kindly make out time to check build status

    Samuel Egbuka
    Team Lead''', subject: 'Check build Status', to: 'samuelegbuka@gmail.com'
    }
}
