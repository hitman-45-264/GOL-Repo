pipeline {
    agent any 
    tools { 
        maven 'Maven' 
      
    }
stages { 
     
 //stage('Preparation') { 
 //   steps {
// for display purpose

      // Get some code from a GitHub repository

   //  git 'https://github.com/sumanthsainooka/GOL-Repo.git'

      // Get the Maven tool.
     
 // ** NOTE: This 'M3' Maven tool must be configured
 
     // **       in the global configuration.   
     //}
   //}

   stage('Build') {
       steps {
       // Run the maven build

      //if (isUnix()) {
         sh 'mvn -Dmaven.test.failure.ignore=true install'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
 
  stage('Unit Test Results') {
      steps {
      junit '**/target/surefire-reports/TEST-*.xml'
      
      }
 }
  
     stage('Artifact upload') {
      steps {
     nexusPublisher nexusInstanceId: '1234', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'gameoflife-web/target/gameoflife.war']], mavenCoordinate: [artifactId: 'gameoflife', groupId: 'com.wakaleo.gameoflife', packaging: 'war', version: '$BUILD_NUMBER']]]
      }
 }
    stage('Deploy War') {
      steps {
          //deploy adapters: [tomcat8(credentialsId: 'tomcat-cred', path: '', url: 'http://172.31.11.23:8080/')], contextPath: null, war: '**/*.war'
          //sh label: '', script: 'ansible-playbook deploy-withinfra.yml'
          sh label: '', script: 'ansible-playbook deploy.yml'
      }
 }
}
post {
        success {
            archiveArtifacts 'gameoflife-web/target/*.war'
        }
        failure {
            mail to:"sumanthsainooka@gmail.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
        }
    }       
}
