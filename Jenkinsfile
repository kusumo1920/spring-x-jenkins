/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'maven:3.9.5-eclipse-temurin-17-focal' } }

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE = 'sqlite'
    }

    stages {
        stage('build') {
            steps {
                echo 'Building'
                sh 'mvn --version'
                sh '''
                    echo "Database engine is ${DB_ENGINE}"
                    echo "DISABLE_AUTH is ${DISABLE_AUTH}"
                    printenv
                '''
            }
        }
        stage('test') {
            steps {
                echo 'Testing'
            }
        }
        stage('deploy - staging') {
            steps {
                echo 'Deploying in staging environment'
                echo './run-smoke-tests'
            }
        }
        stage('sanity check') {
            steps {
                input 'Does the staging environment look ok? If yes, do you wanna proceed to production deployment?'
            }
        }
        stage('deploy - production') {
            steps {
                echo 'Deploying in production environment'
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}