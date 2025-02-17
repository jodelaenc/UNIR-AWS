pipeline {
    agent any

    stage('Get Code') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'UNIR', 
                usernameVariable: 'GIT_USERNAME', 
                passwordVariable: 'GIT_PASSWORD')]) {
                    git url: "https://$GIT_USERNAME:$GIT_PASSWORD@github.com/jodelaenc/UNIR-AWS.git", branch: 'main'
            }
        }
    }
}
