pipeline {
    agent any
    
    stages {
        stage('Get Code') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'UNIR', 
                                                 usernameVariable: 'GIT_USERNAME', 
                                                 passwordVariable: 'GIT_PASSWORD')]) {
                    git url: "https://$GIT_USERNAME:$GIT_PASSWORD@github.com/jodelaenc/UNIR-AWS.git", branch: 'main'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    sam deploy --no-fail-on-empty-changeset --config-file samconfig.toml --config-env production --force-upload
                '''
            }
        }
        stage('Rest Test') {
            steps {
                    sh '''
                        /var/lib/jenkins/.local/bin/pytest -m "readonly" --junitxml=result-rest.xml test/integration/todoApiTest.py
                    '''
                    junit 'result-rest.xml'
            }
        }
    }
}
