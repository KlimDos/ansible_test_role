pipeline {
    environment {
        BUILD_SCRIPTS_GIT = "https://github.com/KlimDos/ansible_test_role.git"
        BUILD_SCRIPTS = 'mypipeline'
        BUILD_HOME = '/home/aalimov/tmp_pipeline_ws'
        http_proxy = "http://proxy-dev.aws.wiley.com:8080"
        https_proxy = "http://proxy-dev.aws.wiley.com:8080"
        NO_PROXY= "169.254.169.254,wiley.com"

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
                sh "virtualenv $WORKSPACE/ansible-lint && source ansible-lint/bin/activate && pip install ansible-lint"
                withSonarQubeEnv('SonarAWS-CT-CMH-backend') 
                {
                    sh "source ansible-lint/bin/activate && \
                    /opt/software/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarScanner/bin/sonar-scanner -X -e \
                        -Dsonar.projectKey=do-cmh-sq-test \
                        -Dsonar.projectName=do-cmh-test \
                        -Dsonar.host.url=http://sonarqube.aws.wiley.com \
                        -Dsonar.login=5e3a9f2e39f222bca65440c615ac3048e2e5a0d4 \
                        -Dsonar.projectBaseDir=$WORKSPACE/repo/$BUILD_SCRIPTS \
                        -Dsonar.sources=."
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