pipeline {
    agent any
    
    tools {
        jdk 'JDK20'
        maven 'M3'
    }
    
    stages {
        stage('Environment') {
            steps {
                sh '''
                    echo "Java version:"
                    java -version
                    echo "Maven version:"
                    mvn -version
                    echo "Workspace contents:"
                    ls -R
                '''
            }
        }
        
        stage('Checkout') {
            steps {
                echo 'Checking out from Git...'
                git branch: 'master', 
                    url: 'https://github.com/RamosER77/SnakeRemake.git'
                sh 'ls -R src/'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh '''
                    echo "Current directory:"
                    pwd
                    echo "Directory contents:"
                    ls -la
                    mvn clean compile
                '''
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                    echo "Test directory contents:"
                    ls -la src/test/java/org/psnbtech/
                    mvn test
                '''
            }
            post {
                always {
                    junit(
                        allowEmptyResults: true,
                        testResults: '**/target/surefire-reports/*.xml'
                    )
                }
            }
        }
    }
    
    post {
        success {
            echo 'Build and tests completed successfully!'
            emailext(
                to: "erubielramos9@gmail.com",
                subject: "SUCCESS: Snake Game Build ${env.BUILD_NUMBER}",
                body: """<p>Build Status: SUCCESS</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p>Check console output at: <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
                    <p>Test Results: All tests passed successfully!</p>""",
                mimeType: 'text/html'
            )
        }
        
        failure {
            echo 'Build or tests failed!'
            emailext(
                to: "erubielramos9@gmail.com",
                subject: "FAILED: Snake Game Build ${env.BUILD_NUMBER}",
                body: """<p>Build Status: FAILED</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p>Check console output at: <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
                    <p>Failed Stage: ${currentBuild.result}</p>""",
                mimeType: 'text/html'
            )
        }
        
        always {
            echo 'Pipeline finished'
        }
    }
}
