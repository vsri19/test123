pipeline {
    agent

        docker { image 'maven:3.3.9-jdk-8' }
    }

    environment {
        MAVEN_OPTS = '-Dhttps.protocols=TLSv1.2 -Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true'
        MAVEN_CLI_OPTS = '--batch-mode --errors --fail-at-end --show-version -DinstallAtEnd=true -DdeployAtEnd=true'
    }

    stages {
        stage('.verify') {
            when {
                not branch 'master'
            }
            steps {
                sh '''mvn $MAVEN_CLI_OPTS verify'''
            }
        }
        stage('verify:jdk8') {
            when {
                not branch 'master'
            }
            steps {
                sh '''mvn $MAVEN_CLI_OPTS verify'''
            }
        }
        stage('deploy:jdk8') {
            when {
                branch 'master'
            }
            steps {
                sh '''if [ ! -f ci_settings.xml ]; then echo "CI settings missing! If deploying to GitLab Maven Repository, please see https://docs.gitlab.com/ee/user/project/packages/maven_repository.html#creating-maven-packages-with-gitlab-cicd for instructions."; fi'''
                sh '''mvn $MAVEN_CLI_OPTS deploy -s ci_settings.xml'''
            }
        }
    }

