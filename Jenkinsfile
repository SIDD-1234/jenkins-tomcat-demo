pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading source code from GitHub'

                git branch: 'main',
                    url: 'https://github.com/SIDD-1234/jenkins-tomcat-demo'
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling Java application'

                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Executing JUnit tests'

                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Creating WAR file'

                sh 'mvn package -DskipTests'

                archiveArtifacts artifacts: 'target/*.war',
                                 fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to Tomcat'

                sh '''
                    cp target/jenkins-demo.war \
                    /opt/homebrew/opt/tomcat/libexec/webapps/jenkins-demo.war
                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Checking deployed application'

                sh '''
                    curl --fail \
                    http://localhost:8081/jenkins-demo/
                '''
            }
        }
    }

    post {

        always {
            junit allowEmptyResults: true,
                  testResults: 'target/surefire-reports/*.xml'
        }

        success {
            echo 'CI/CD PIPELINE SUCCESSFUL'
        }

        failure {
            echo 'CI/CD PIPELINE FAILED'
        }
    }
}
