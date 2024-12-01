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
                '''
            }
        }
        
        stage('Checkout') {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/RamosER77/SnakeRemake.git'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
    }
    
    post {
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed!'
        }
    }
}
