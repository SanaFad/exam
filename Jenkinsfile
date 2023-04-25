pipeline {
    environment {
        registry = "SanaFad/exam"
        registryCredential = 'dockerhub_id'
        dockerImage = 'exam'
    }
    agent any
    stages {
        stage('Git') {
            steps {
                echo "Getting Project from Git";
                sh "rm -rf exam"
                sh "git clone https://github.com/SanaFad/exam.git"
                  }
            }
            
        stage('MVN Clean'){  
            steps {
                sh "mvn clean"
                  }
        }        
        
        stage('MVN Install'){
            steps {
                sh "mvn install"
                  }
        }       
        
        stage('MVN Test'){
            steps {
                sh "mvn test"
                  }
        }          
        
        stage('MVN Package'){
            steps {
                sh "mvn package"
                  }
        }          
        
        stage('MVN SONARQUBE'){
            steps{
                sh 'mvn sonar:sonar \
  -Dsonar.projectKey=JenkinsExamProject \
  -Dsonar.host.url=http://192.168.1.120:9000 \
  -Dsonar.login=47e5a816defea731387d72ad9707363b75676b29'
                 }
        }    
        stage('MVN NEXUS'){
            steps {
                sh 'mvn deploy -Dmaven.test.skip=true'
                  }
        }
        stage('Building our image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy our image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
   }
}
