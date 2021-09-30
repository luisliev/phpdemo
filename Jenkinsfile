pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                    containers:
                    - name: phpunit
                      image: phpunit/phpunit
                      command:
                      - cat
                      tty: true
            '''
        }
    }
    stages {
        stage('Unit Tests') {
            steps {
                container('phpunit') {
                    sh './vendor/bin/phpunit --bootstrap src/autoload.php --coverage-html .'
                }
            }
        }
    }
}