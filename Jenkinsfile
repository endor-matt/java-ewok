pipeline {
    agent any
    tools {
        maven "maven3"
        jdk "openjdk8"
    }

    environment {
        SNAP_REPO = 'mb-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'Dazz123'
        RELEASE_REPO = 'mb-release'
        CENTRAL_REPO = 'mb-maven-central'
        NEXUSIP = '172.31.11.68' //private ip since in the same network
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'mb-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now archiving"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
    }
}
