pipeline {
    agent any
    
    stages {
        stage('Get Code') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'UNIR', 
                                                 usernameVariable: 'GIT_USERNAME', 
                                                 passwordVariable: 'GIT_PASSWORD')]) {
                    git url: "https://$GIT_USERNAME:$GIT_PASSWORD@github.com/jodelaenc/UNIR-AWS.git", branch: 'develop'
                }
            }
        }
        stage('Static Test'){
            steps{
                sh '''
                    bandit --version
                    flake8 --format=pylint --exit-zero app > flake8.out
                    bandit -r . -f custom -o bandit.out --msg-template "{abspath}:{line}: {severity}: {test_id}: {msg}"
                '''
                recordIssues tools: [flake8(name: 'Flake8', pattern: 'flake8.out')], qualityGates: [[threshold:8, type: 'TOTAL', unstable: true], [threshold: 10, type: 'TOTAL', unstable: false]]
                recordIssues tools: [pyLint(name: 'Bandit', pattern: 'bandit.out')], qualityGates: [[threshold:2, type: 'TOTAL', unstable: true], [threshold: 4, type: 'TOTAL', unstable: false]]
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    sam deploy --no-fail-on-empty-changeset --config-file samconfig.toml --config-env staging --force-upload
                '''
            }
        }
        stage('Rest Test') {
            steps {
                    sh '''
                        
                        /var/lib/jenkins/.local/bin/pytest --junitxml=result-rest.xml test/integration/todoApiTest.py
                    '''
                    junit 'result-rest.xml'
            }
        }
        stage('Promote') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'UNIR', 
                    usernameVariable: 'GIT_USERNAME', 
                    passwordVariable: 'GIT_PASSWORD')]) {
                        sh '''
                            git checkout main
                            git pull
                            git merge develop
                            git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/jodelaenc/UNIR-AWS.git main
                        '''
                }
            }
        }
    }
}
