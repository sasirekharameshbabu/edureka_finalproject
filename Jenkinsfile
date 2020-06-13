pipeline{
    agent none
        stages{
            stage('checkout'){
                agent any
                steps{
                    git 'https://github.com/sasirekharameshbabu/edureka_finalproject.git'
                }
            }
            
            stage('Docker_Build'){
                agent any
                steps{
                    sh 'sudo docker build -t prsasirekha/edureka_final_project:latest .'
                }
            }
            
            stage('Docker_Push'){
                agent any
                steps{
                    withCredentials([string(credentialsId: 'password', variable: 'password')]) {
                        sh 'sudo docker login -u prsasirekha -p $password'
                    }
                
                    sh 'sudo docker push prsasirekha/edureka_final_project:latest'
            }
            }
            stage('Docker_CleanUP'){
                agent any
                steps{
                    sh 'sudo docker ps -f name=mycont -q | xargs --no-run-if-empty sudo docker container stop'
                    sh 'sudo docker container ls -a -fname=mycont -q | xargs -r sudo docker container rm'
                }
            }
            stage('Docker_Run'){
                agent any
                steps{
                    sh 'sudo docker run --name mycont -d -p 8140:80 prsasirekha/edureka_final_project:latest'
                }
            }
            
            stage('Selenium_Test'){
                agent {label'windows_slave'}
                steps{
                    git 'https://github.com/sasirekharameshbabu/edureka_finalproject.git'
                    bat 'mvn test'
                }
            }            
            
        }   
}
