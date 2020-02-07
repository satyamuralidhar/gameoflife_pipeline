pipeline {
    agent any 
    
           
        
    stages {
         stage("scm using git"){
        
            steps {
                git credentialsId: '284cf0a3-3286-44b7-800e-4e84a4072364', url: 'https://github.com/satyamuralidhar/game-of-life.git'
            }
    
        } 
        
        stage("Maven Build") {
            steps {
                sh label: '', script: 'mvn package'
            }
        }   
    
        stage("mail config") {
            steps {
                emailext body: '', recipientProviders: [developers()], subject: 'build', to: 'muralidhar.peddireddi@gmail.com'
            }    
        
        }
        stage("archive artifact") {
            steps {
                archiveArtifacts '**/*.war'
            }
        }
        stage('Artifactory configuration') {
            steps {
                rtServer (
                    id: "JFROG",
                    url: "http://192.168.222.135:8081/artifactory/",
                    //credentialsId: JFROG
                    username: 'admin',
                    password: '@MUrali5'
                )
                 rtMavenDeployer (
                    id: "JFROG",
                    serverId: "JFROG",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )
            }
        }    

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG"
                )
            }
        }
        stage('SonarQube analysis') {
    // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
            steps {
                 withSonarQubeEnv('sonar') {
      // requires SonarQube Scanner for Maven 3.2+
                     sh 'mvn clean package sonar:sonar'
                 }    
                  
            }
        }     
        stage('Junit Test') {
            steps {
                junit '**/*.xml'
            }
        }    
    
      
        
    
    }
    
}
