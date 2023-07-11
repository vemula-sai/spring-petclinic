pipeline {
    agent { label 'OPEN' }
    stages {
        stage ('vcs') {
            steps {
                git url: 'https://github.com/vemula-sai/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "jfrog-jul11",
                    url: "https://latestqt.jfrog.io/",
                    credentialsId: "jfrog-jul11"
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "jfrog-jul11",
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
                    serverId: "jfrog-jul11"
                )
            }
        }

    }
}