pipeline {
    agent any
     
        tools { 
        maven 'Maven 3.6.3'
             }
    
    
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
        
       stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/BezzineOnsa/Devops-Project.git'
                }
            }
        }

      stage('maven'){
            steps{
                sh "mvn -version"
            }
        }
        
         stage('Build') {
            steps {
                    sh "mvn install"
                    //sh "mvn compile"
            }
        }
   
          stage('Unit Testing'){
            steps{
                sh "docker --version"
            }
        }
        stage ('SRC Analysis Testing SonarQube'){
            steps {

           sh "mvn sonar:sonar -Dsonar.login=admin  -Dsonar.host.url=http://192.168.33.10:9000/ -Dsonar.password=sonar"
            }

         }
          stage('Build Artifact'){
            steps {
                sh 'mvn clean install'
            }

        }
         stage('Compile Project'){
            steps {
                sh 'mvn compile -DskipTests'
            }
        }



               stage('Docker Build') {
            steps {
               
      	           sh "docker build -t onsa/project ."
                
            }
        }
        
       stage("Publish to Nexus Repository Manager") {
            steps {
                sh  "mvn deploy:deploy-file -DgroupId=tn.esprit -DartifactId=ExamThourayaS2 -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://192.168.33.10:8081/repository/maven-releases/ -Dfile=target/ExamThourayaS2-0.0.1-SNAPSHOT.jar -DskipTests" 
            }
        }


        stage("Login to DockerHub") {
                steps{

                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u onsa -p 13633447Onsa'
                }
        }
     /*    stage("Deploy Image to DockerHub") {
                steps{
                    sh 'docker push jawher123456/project'
                }
        }*/

        
        stage('Start container') {
            steps {
                 sh "docker compose up -d"

            }
        }   

        

         
    }
}