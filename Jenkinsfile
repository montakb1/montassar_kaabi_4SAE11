pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    stages {
        stage('Hello') {
            steps {
                echo'Hello World'
            }
        }   
    }
    stages {
        stage('GIT') {
            steps {
                git branch: 'main',
                url: 'https://github.com/montakb1/montassar_kaabi_4SAE11.git'
            }
        }   
    }
}

