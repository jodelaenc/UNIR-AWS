pipeline {
    agent any
    
    stages {
        stage('Get Code') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'UNIR', 
                                                 usernameVariable: 'GIT_USERNAME', 
                                                 passwordVariable: 'GIT_PASSWORD')]) {
                    git url: "https://$GIT_USERNAME:$GIT_PASSWORD@github.com/jodelaenc/UNIR-AWS.git", branch: 'main'
                    dir('config-repo') {
                            git url: "https://$GIT_USERNAME:$GIT_PASSWORD@github.com/jodelaenc/UNIR-AWS-CONFIG.git", branch: 'production'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    cp config-repo/samconfig.toml samconfig.toml
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
<<<<<<< HEAD
=======
        stage('Promote') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'UNIR', 
                    usernameVariable: 'GIT_USERNAME', 
                    passwordVariable: 'GIT_PASSWORD')]) {
                        sh '''
                            git checkout main
                            git pull origin main
                            git merge develop --no-ff -m "Promoción automática desde develop a main"
                            git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/jodelaenc/UNIR-AWS.git main
                        '''
                }
            }
        }
>>>>>>> develop
    }
}
