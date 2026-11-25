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
        
        stage('Sonarqube Analysis'){
 
            steps{
               
               bat ''' mvn clean verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
               -Dsonar.projectKey=Spring-Petclinic \
               -Dsonar.projectName='Spring-Petclinic' \
               -Dsonar.host.url=http://localhost:9000 \
               -Dsonar.token=sqp_4a58f58d5b4cd1dd1544bbf761c344b9167521c7'''
            }
        }
        
        stage('Published Artifcats'){
 
            steps{
 
                archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
            }
        }

	stage ('Artifactory Server'){

    steps {
    
       rtServer (
       
         id: "Artifactory",
         
         url: 'http://localhost:8081/artifactory',
         
         username: 'admin',
         
          password: 'password',
          
          bypassProxy: true,
          
           timeout: 300
           
                )
    }
    
}
stage('Upload'){

    steps{
    
        rtUpload (
        
         serverId:"Artifactory" ,
         
          spec: '''{
          
           "files": [
           
              {
              
              "pattern": "*.war",
              
              "target": "VeerendraChilukuri"
              
              }
              
                    ]
                    
                   }''',
                   
                )
    }
    
}
stage ('Publish build info') {

    steps {
    
        rtPublishBuildInfo (
        
            serverId: "Artifactory"
        )
        
    }
    
}
        
        stage('Deployment'){
 
            steps{
 
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'TomcatCredentials', path: '', url: 'http://localhost:8080/')], contextPath: 'PetClinic', war: 'target/*.war'
            }
        }
    }
}