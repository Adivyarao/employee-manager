pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Manual Checkout') {
            steps {
                sh 'rm -rf employee-manager || true'
                sh 'git clone -b deploy-war https://github.com/Adivyarao/employee-manager.git'
            }
        }

        stage('Build') {
            steps {
                dir('employee-manager') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                dir('employee-manager') {
                    sh '''
                        echo "Copying WAR to Tomcat container..."
                        docker exec -i webserver sh -c 'cat > /usr/local/tomcat/webapps/employee-manager.war' < target/employee-manager-1.0.0.war
                    '''
                }
            }
        }

        stage('Archive') {
            steps {
                dir('employee-manager') {
                    archiveArtifacts artifacts: 'target/*.war', fingerprint: true
                }
            }
        }
    }
}

