pipeline {
    environment {
        SONAR_LOGIN = credentials('sonar-cloud-token')
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
                    - name: php
                      image: php:7.4.24-cli
                      command:
                      - cat
                      tty: true
                    - name: sonar-scanner
                      image: sonarsource/sonar-scanner-cli
                      command:
                      - cat
                      tty: true
                      env:
                        - name: SONAR_LOGIN
                          value: '$SONAR_LOGIN'
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
                container('php') {
                    sh './vendor/bin/phpunit --bootstrap src/autoload.php'
                    sh 'ls -la tests/'
                }
            }
            post {
                always {
                    archiveArtifacts artifacts: 'tests/reports/**/*', fingerprint: true
                    junit 'tests/reports/**/*.xml'
                }
            }
        }
        stage('Sonar') {
            steps {
                echo '===== Running Sonar Analysis ====='
                container('sonar-scanner') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
            // steps {
            //     waitForQualityGate abortPipeline: true
            // }
        }
    }
}