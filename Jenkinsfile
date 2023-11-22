pipeline{
    agent any
    tools{
       
        maven 'maven3.9'
    }

     environment{
        SCANNER_HOME= tool 'sonar-server' 
    }
    
    stages{
        stage('GitCheckout'){
            steps{
                git branch: 'main', url: 'https://github.com/jaiswaladi246/Ekart.git'
            }
        }

        stage('Compile'){
            steps{
                   sh "mvn clean compile -DskipTests=true"
            }
        }

         stage('Sonarqube') {
            steps {
                withSonarQubeEnv('sonar-server'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Shopping-Cart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=Shopping-Cart '''
               }
            }
        }

           stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }

          stage('Docker Build & Push') {
            steps {
                script{ withDockerRegistry(credentialsId: '0454a329-3aef-46e5-95c9-fd05bbb38519', toolName: 'Docker') {
                      sh "docker build -t shopping-cart -f docker/Dockerfile ."
                        sh "docker tag  shopping-cart faizandocker29/faizan-labs:latest"
                        sh "docker push faizandocker29/faizan-labs:latest"
                    }
                }
            }
        }

         stage('Deploy') {
            steps {
                script{ withDockerRegistry(credentialsId: '0454a329-3aef-46e5-95c9-fd05bbb38519', toolName: 'Docker')
                {
                    sh "docker run -d --name shop-shop -p 8070:8070  faizandocker29/faizan-labs:latest"
                }
            }
        }
        
        
         }
         

                

         
    }
}
