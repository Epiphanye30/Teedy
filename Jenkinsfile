pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('pmd') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }
        stage('Generate Javadoc') {
            steps {
                sh 'mvn site --fail-never'
            }
        }
        
    }
    post {
            always {
                archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
                archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
                archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
                archiveArtifacts artifacts: '**/target/site/apidocs', fingerprint: true
            }
        }
}
