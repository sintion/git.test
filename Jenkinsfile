pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('Install packages') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'nohup npm start &'
                sh 'sleep 10'
                sh 'curl -k localhost:3000'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
                sh 'tar -czvf build.tar.gz build/*'
                archiveArtifacts artifacts: 'buid.tar.gz', followSymlinks: fals
            }
        }
    }
}