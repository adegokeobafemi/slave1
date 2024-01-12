pipeline {
    tools {
        jdk 'myjava'
        maven 'mymaven'
    }
    
    agent any
    
    stages {
        stage('Checkout') {
            agent {
                label 'Master'
            }
            steps {
                echo 'Cloning...'
                git 'https://github.com/theitern/ClassDemoProject.git'
            }
        }
        
        stage('Compile') {
            agent {
                label 'Slave_1'
            }
            steps {
                echo 'Compiling...'
                sh 'mvn compile'
            }
        }
        
        stage('CodeReview') {
            agent {
                label 'Slave_1'
            }
            steps {
                echo 'Code Review...'
                sh 'mvn pmd:pmd'
            }
        }
        
        stage('UnitTest') {
            agent {
                label 'Slave_1'
            }
            steps {
                echo 'Testing...'
                sh 'mvn test'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            agent {
                label 'Master'
            }
            steps {
                echo 'Packaging...'
                sh 'mvn package'
            }
        }
    }
}

