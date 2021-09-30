pipeline {
    environment {
        SONAR_TOKEN = credentials('sonar-cloud-token')
    }
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                    containers:
                    - name: composer
                      image: composer:2.1.8
                      command:
                      - cat
                      tty: true
                    - name: xdebug
                      image: mileschou/xdebug:8.0
                      command:
                      - cat
                      tty: true
                      env:
                        - name: XDEBUG_MODE
                          value: coverage
                    - name: sonar-scanner
                      image: sonarsource/sonar-scanner-cli
                      command:
                      - cat
                      tty: true
                      env:
                        - name: SONAR_TOKEN
                          value: $SONAR_TOKEN
            '''
        }
    }
    stages {
        stage('Build') {
            steps {

                echo '===== Installing Dependencies ====='
                container('composer') {
                    sh 'composer install'
                }

                echo '===== Running Unit Tests ====='
                container('xdebug') {
                    sh './vendor/bin/phpunit'
                    sh 'ls -la tests/'
                }

                echo '===== Running Sonar Analysis ====='
                container('sonar-scanner') {
                    withSonarQubeEnv('SonarCloud') {
                        sh "sonar-scanner"
                    }
                }
            }
            post {
                always {
                    // archiveArtifacts artifacts: 'tests/**/*', fingerprint: true
                    junit 'tests/junit.xml'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}