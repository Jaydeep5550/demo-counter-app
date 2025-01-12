pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/Jaydeep5550/demo-counter-app.git'
                }
            }
        }
        stage('UNIT testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                }
                    
                }
            }
            stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
            stage('Upload war file to nexus'){
                
                steps{
                    
                    script{
                       
                        
                        nexusArtifactUploader artifacts: 
                        [
                            [
                                artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar'
                            ]
                        ], 
                            credentialsId: 'nexus-auth',
                            groupId: 'com.example', 
                            nexusUrl: '3.110.148.223:8081', 
                            nexusVersion: 'nexus3', 
                            protocol: 'http', 
                            repository: 'Demo', 
                            version: '1.0.1'
                        }
                    }
               }
                stage('Docker Image Build'){
                   
                    steps{
                        
                        script{
                            sh 'docker image build -t $JOB_NAME:v1.BUILD_ID'
                            sh 'docker image tag $JOB_NAME:v1.BUILD_ID jaydeep9555/$JOB_NAME:v1.BUILD_ID'
                            sh 'docker image tag $JOB_NAME:v1.BUILD_ID jaydeep9555/$JOB_NAME:latest'
                       }
                    }
                }
            }
        
        }
