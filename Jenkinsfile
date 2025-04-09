pipeline {
    agent any

    tools {
        maven 'Maven 3.9.9'
        jdk 'Java 17'
    }

    environment {
        SERVICES = 'spring-petclinic-visits-service spring-petclinic-vets-service spring-petclinic-customers-service'
    }

    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com/QuyDang1108/spring-petclinic-microservices.git', branch: 'main'
            }
        }

        stage('Build, Test & Coverage per Service') {
            steps {
                script {
                    def services = env.SERVICES.split()
                    services.each { svc ->
                        echo ">>> Building and testing ${svc}"
                        dir("${svc}") {
                            sh 'mvn clean verify'
                        }
                    }
                }
            }
        }

        stage('Publish Coverage Report (example: visits-service)') {
            steps {
                jacoco execPattern: 'spring-petclinic-visits-service/target/jacoco.exec',
                       classPattern: 'spring-petclinic-visits-service/target/classes',
                       sourcePattern: 'spring-petclinic-visits-service/src/main/java',
                       exclusionPattern: '**/test/**'
            }
        }
    }

    post {
        always {
            echo "✅ Pipeline finished."
        }
    }
}
