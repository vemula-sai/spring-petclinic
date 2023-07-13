pipeline {
    agent { label 'OPEN' }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/vemula-sai/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('SONAR') {
            steps {
                withSonarQubeEnv('sonarqube') {
                sh 'mvn clean package sonar:sonar'
              }
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "JFROG_CONFIG",
                    url: "https://latestqt.jfrog.io/",
                    credentialsId: "JFROG_TOKEN"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "JFROG_CONFIG",
                    releaseRepo: "qt-libs-release-local",
                    snapshotRepo: "qt-libs-snapshot-local"
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: "maven_home", // Tool name from Jenkins configuration
                    pom: "pom.xml",
                    goals: "clean install",
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG_CONFIG"
                )
            }
        }

    }
}
