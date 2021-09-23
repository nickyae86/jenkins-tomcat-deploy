pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/nickyae86/jenkins-tomcat-deploy'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.war'
                }
            }
        }
        stage('deploy'){
            steps{
                deploy adapters: [tomcat8(credentialsId: '0584ff57-d327-4568-bb51-ddbbd0f05923', path: '', url: 'http://192.168.0.118:9090/')], contextPath: '/springboot', war: '**/*.war'
            }
        }
    }
}