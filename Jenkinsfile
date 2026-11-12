pipeline{
    agent any
    stages{
        stage('Clone'){
            steps{
              git branch: 'feature/2026.11.12', url: 'https://github.com/veerendrachilukuri/Petclinic.git'
            }
        }
        stage('Build'){
            steps{
                bat 'mvn install'
            }
        }
        stage('Test'){
 
            steps{
 
                bat 'mvn test'
            }
        }
 
        stage('Generated Tests Reports'){
 
            steps{
                junit 'target/surefire-reports/*.xml'
            }
        }
        stage('Published Artifcats'){
 
            steps{
 
                archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }
        
        stage('Deployment'){
 
            steps{
 
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'TomcatCredentials', path: '', url: 'http://localhost:8080/')], contextPath: 'PetClinic', war: 'target/*.war'
            }
        }
    }
}