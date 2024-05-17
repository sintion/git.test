pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
        stage('install packages') {
            steps {
                sh 'npm install'
            }
        }
        stage('test') {
            steps {
                sh 'nohup npm start &'
                sleep 10
                sh 'curl -k localhost:3000'
            }
        }
        stage('build app') {
            steps {
                sh 'npm run build'
                sh 'tar -czvf build.tar.gz build/*'
                archiveArtifacts artifacts: 'build.tar.gz', followSymlinks: false
                
            }
        } 
        // stage('publish docker img')   
        stage('build docker image'){
            steps{
                sh 'docker build -t appimg .'
            }
        }    
       // stage('publish docker image'){
        //    steps{
        //        sh 'docker tag appimg appimg:1.0.0'
        //        sh 'docker push sintion/appimg:1.0.0'
        //    }    
        //}
        stage('cleanup container'){
            steps{
                sh '''
                    if docker container ls -a | grep app ;
                    then
                        docker container stop app
                        docker container rm app
                    fi
                '''        
                }
            }
        stage('deploy container'){
            steps{
                sh 'docker run -d -p 4000:80 --name app appimg'
                sleep 10
                sh 'curl -k localhost:4000'
            }
        }
    }
}
