pipeline {
    environment {
        BUILD_SCRIPTS_GIT = "https://github.com/KlimDos/ansible_test_role.git"
        BUILD_SCRIPTS = 'mypipeline'
        BUILD_HOME = '/home/aalimov/tmp_pipeline_ws'
    }
    agent any
    stages {
        stage('Checkout: Code') {
            steps {
                sh "mkdir -p $WORKSPACE/repo;\
                    git clone $BUILD_SCRIPTS_GIT repo/$BUILD_SCRIPTS"
                sh "chmod -R +x $WORKSPACE/repo/$BUILD_SCRIPTS"
            }
        }
        stage('Yum: Updates') {
            steps {
                sh "sudo ls -la $WORKSPACE/repo/$BUILD_SCRIPTS/"
            }
        }
        stage('Post to Sonar') {
            steps {
                withSonarQubeEnv('SonarAWS-CT-CMH-backend') {
                    sh "/opt/software/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScanner/bin/sonar-scanner"
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}