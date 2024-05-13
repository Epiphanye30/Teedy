// pipeline {
//     agent any
//     stages {
//         stage('Build') {
//             steps {
//                 sh 'mvn -B -DskipTests clean package'
//             }
//         }
//         stage('pmd') {
//             steps {
//                 sh 'mvn pmd:pmd'
//             }
//         }
//         stage('Generate Javadoc') {
//             steps {
//                 sh 'mvn site --fail-never'
//             }
//         }
        
//     }
//     post {
//             always {
//                 archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
//                 archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
//                 archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
//             }
//         }
// }

pipeline {
    agent any
        stages{
        stage('Package') {
            steps {
            checkout scmGit(branches: [[name: '*/master']], extensions: [],
            userRemoteConfigs: [[url: 'https://github.com/Epiphanye30/Teedy.git']])
            sh 'mvn -B -DskipTests clean package'
            }
        }
        // Building Docker images
            stage('Building image') {
            steps{
                //your command
                sh 'docker build --tag teedy .'
            }
        }
        // Uploading Docker images into Docker Hub
            stage('Upload image') {
            steps{
                //your command
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                    sh "docker tag teedy ${DOCKERHUB_USERNAME}/teedy"
                    sh "docker push ${DOCKERHUB_USERNAME}/teedy"
                }
            }
        }
        //Running Docker container
        stage('Run containers'){
            steps{
                //your command
                sh 'docker run -d -p 8080:8080 $DOCKERHUB_USERNAME/teedy'
                sh 'docker run -d -p 8081:8081 $DOCKERHUB_USERNAME/teedy'
                sh 'docker run -d -p 8082:8082 $DOCKERHUB_USERNAME/teedy'
            }
        }
    }
}