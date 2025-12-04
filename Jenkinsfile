pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
        // Note: SonarQube token is handled differently with withSonarQubeEnv
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'master',
                    url: 'git@github.com:Therealcydex/Ahmedwassim_hammouda_4SAE11.git'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean compile -DskipTests'
            }
        }

     

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sq1') { 
                    sh '''
                        mvn sonar:sonar \
                            -Dsonar.projectKey=student-management \
                            -Dsonar.projectName="Student Management"
                    '''
                }
                
            }
        }

        stage('Quality Gate Check') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    def dockerImage = 'wassimhamouda/student-management'
                    def dockerTag = "${dockerImage}:${env.BUILD_NUMBER}"
                    sh "docker build -t $dockerTag -t $dockerImage:latest ."
                    sh "docker push $dockerTag"
                    sh "docker push $dockerImage:latest"
                }
            }
        }
    }
}
