pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ghogrerahul/argocd-example-apps.git', branch: 'main', credentialsId: 'github-creds'
            }
        }
        stage('Build & Test') {
            steps {
                sh 'echo "Run unit tests here"'
            }
        }
        stage('Helm Lint & Package') {
            steps {
                sh 'helm lint helm-guestbook'
                sh 'helm package helm-guestbook'
            }
        }
        stage('Push to GitOps Repo') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    git config user.email "jenkins@example.com"
                    git config user.name "Jenkins"
                    git add .
                    git commit -m "Update Helm chart"
                    git push https://$USER:$PASS@github.com/ghogrerahul/argocd-example-apps.git main
                    '''
                }
            }
        }
    }
}
