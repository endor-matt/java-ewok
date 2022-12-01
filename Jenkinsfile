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
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
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
                sh 'mvn -s settings.xml test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
        
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }
        }

        stage("Quality Gate") {
            steps { 
                timeout(time: 1, unit: 'HOURS')
                // Parameter indicates whether to set pipeline to unstable
                // true = set pipeline to unstable, false = don't
                waitForQualityGate abortPipeline: true
            }
        }
    }
}
