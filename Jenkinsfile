// pipeline {
//     agent { label 'node-agent' }
    
//     stages{
//         stage('Code'){
//             steps{
//                 git url: 'https://github.com/LondheShubham153/node-todo-cicd.git', branch: 'master' 
//             }
//         }
//         stage('Build and Test'){
//             steps{
//                 sh 'docker build . -t trainwithshubham/node-todo-test:latest'
//             }
//         }
//         stage('Push'){
//             steps{
//                 withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
//         	     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
//                  sh 'docker push trainwithshubham/node-todo-test:latest'
//                 }
//             }
//         }
//         stage('Deploy'){
//             steps{
//                 sh "docker-compose down && docker-compose up -d"
//             }
//         }
//     }
// }



pipeline {
    agent { label 'node-agent' }
    
    stages {
        stage('Code') {
            steps {
                git url: 'https://github.com/aishwarya-blocsys/jenkins-todo-app.git', branch: 'main'
            }
        }
        stage('Test') {
            steps {
                sh 'npm install' // install dependencies
                sh 'npm  run test' // run tests
            }
        }
        stage('Build') {
            steps {
                sh 'docker build . -t aishwarya-blocsys/jenkins-todo-app:latest'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh 'docker push aishwarya-blocsys/jenkins-todo-app:latest'
                }
            }
        }
        stage('Deploy') {
            when {
                branch 'main' // deploy only when merging to the main branch
            }
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            // Trigger the deployment process after the pull request is merged
            build job: 'deploy-my-app'
        }
    }
}
