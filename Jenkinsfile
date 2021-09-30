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
        stage('Unit Tests') {
            steps {
                container('php') {
                    sh './vendor/bin/phpunit --bootstrap src/autoload.php --coverage-html .'
                }
            }
        }
    }
}