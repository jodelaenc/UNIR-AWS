pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                    git branch: 'develop', url: 'https://github.com/jodelaenc/UNIR-AWS.git'
                    bat 'dir'
                    echo WORKSPACE
            }
        }
    }
}
