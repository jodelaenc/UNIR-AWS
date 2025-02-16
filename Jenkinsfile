pipeline {
    agent any

    stages {
        stage('Git') {
            steps {
                git branch: 'develop', url: 'https://github.com/jodelaenc/UNIR-AWS.git'
            }
        }
        stage('Build') {
            steps {
                echo 'No compila'
                bat "dir"
            }
        }
    }
}
