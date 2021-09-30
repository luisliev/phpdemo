pipeline {
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
                    sh './vendor/bin/phpunit --bootstrap src/autoload.php --coverage-html .'
                }
            }
        }
    }
}