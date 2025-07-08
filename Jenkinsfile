
pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Adivyarao/employee-manager.git', credentialsId: 'github-creds', branch: 'deploy-war'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    echo "Copying WAR to Tomcat container..."
                    docker exec -i webserver sh -c 'cat > /usr/local/tomcat/webapps/employee-manager.war' < "$WORKSPACE/target/employee-manager-1.0.0.war"
                '''
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }
    }
}

