pipeline {
    agent any // use any agent

    stages {
        stage('Git') {
            steps {
                //get source code from branch
                    git branch: 'rayenOueslati_5TWIN4_G1',
                        url: 'https://github.com/SarraTounsi/5TWIN4-G1-kadeem'
        
            }
        }
         stage('Build with Maven') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.login=admin -Dsonar.password=rayen '
            }
        }
        stage('MVN TEST JUNIT') {
            steps {
                sh 'mvn test'
            }
        }
        stage('credentials during build') {
            steps {
                sh 'mvn help:effective-settings -DshowPasswords=true'
            }
        }
        // stage('MVN DEPLOY TO NEXUS') {
        //     steps {
        //         sh 'mvn deploy -Dmaven.test.skip=true'
        //     }
        // }
         stage('Docker Image') {
            steps {
                sh 'sudo docker build -t rayenoueslati-5twin4-g1 .'
            }
        }
        // stage('Docker Image Push') {
        //     steps {
        //         script {
        //             // sh 'sudo docker login --username "rayen15" --password-stdin'
        //             sh "docker login -u $DOCKER_HUB_USERNAME -p $DOCKER_HUB_PASSWORD"
        //             sh 'sudo docker tag rayenoueslati-5twin4-g1 rayen15/devops:latest'
        //             sh 'sudo docker push rayen15/devops:latest'
        //         }
        //     }
        // }
          stage('Push Docker Image') {
            steps {
                script {
                    def dockerHubToken = credentials('DOCKERHUB_ACCESS_TOKEN')

                    // Log in to Docker Hub using the token
                    withCredentials([string(credentialsId: 'DOCKERHUB_ACCESS_TOKEN', variable: 'DOCKERHUB_TOKEN')]) {
                        sh "sudo docker login -u rayen15 -p $DOCKERHUB_TOKEN"
                    }

                    // Push the image to Docker Hub
                    sh 'sudo docker tag rayenoueslati-5twin4-g1 rayen15/devops:latest'
                    sh 'sudo docker push rayen15/devops:latest'
                }
            }
        }
         stage('Docker Compose') {
            steps {
                sh 'sudo docker compose up -d'
            }
        }

      
    }
    post {
        success {
            echo 'Build successful'
        }
        failure {
            echo 'fail'
        }
    }
}
