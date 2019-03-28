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
    }
    post {
        always {
            cleanWs()
        }
    }
}