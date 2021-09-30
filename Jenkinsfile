pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                    containers:
                    - name: php
                      image: php:7.4.24-cli
                      command:
                      - cat
                      tty: true
            '''
        }
    }
    stages {
        stage('Build') {
            steps {
                
                echo '===== Installing Dependencies ====='
                container('php') {
                    sh 'composer install'
                }

                echo '===== Running Unit Tests ====='
                container('php') {
                    sh './vendor/bin/phpunit --bootstrap src/autoload.php --coverage-html .'
                }
            }
        }
    }
}