pipeline {
    agent {
        docker { image 'maven:3.3.9-jdk-8' }
    }

    environment {
        MAVEN_OPTS = '-Dhttps.protocols=TLSv1.2 -Dmaven.repo.local=${env.WORKSPACE}/.m2/repository -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true'
        MAVEN_CLI_OPTS = '--batch-mode --errors --fail-at-end --show-version -DinstallAtEnd=true -DdeployAtEnd=true'
    }

    stages {
        stage('Verify Workspace') {
            steps {
                script {
                    sh 'pwd'
                    sh 'ls -l'
                }
            }
        }
        stage('Verify') {
            when {
                not { branch 'master' }
            }
            steps {
                sh '''mvn $MAVEN_CLI_OPTS verify'''
            }
        }
        stage('Verify JDK8') {
            when {
                not { branch 'master' }
            }
            steps {
                sh '''mvn $MAVEN_CLI_OPTS verify'''
            }
        }
        stage('Deploy JDK8') {
            when {
                branch 'master'
            }
            steps {
                script {
                    if (!fileExists('ci_settings.xml')) {
                        error "CI settings missing! If deploying to GitLab Maven Repository, please see GitLab documentation for instructions."
                    }
                }
                sh '''mvn $MAVEN_CLI_OPTS deploy -s ci_settings.xml'''
            }
        }
    }
}
