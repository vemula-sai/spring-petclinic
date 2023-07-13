pipeline {
    agent { label 'SONAR' }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/vemula-sai/spring-petclinic.git',
                    branch: 'sonar'
            }
        }
        stage ('sonar') {
            steps {
                withSonarQubeEnv('sonar') {
                sh 'mvn clean package sonar:sonar'
              }
            }
        }
    }
}
