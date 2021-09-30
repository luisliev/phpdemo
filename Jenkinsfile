pipeline {
    agent none
    stages {
        podTemplate {
            node('phpunit') {
                stage('Unit Tests') {
                    steps {
                        container('phpunit') {
                            ./phpunit --bootstrap src/autoload.php --coverage-html .
                        }
                    }
                }
            }
        }
    }
}