pipeline {
    agent none

    environment {
        MY_JAVA = 'myjava'
        MY_MAVEN = 'mymaven'
    }

    tools {
        jdk MY_JAVA
        maven MY_MAVEN
    }

    stages {
        stage('Checkout') {
            agent any
            steps {
                echo 'Cloning...'
                git 'https://github.com/theitern/DevOpsCodeDemo.git'
            }
        }

        stage('Compile') {
            agent { label 'Slave_1' }
            steps {
                echo 'Compiling...'
                sh 'mvn compile'
            }
        }

        stage('CodeReview') {
            agent { label 'Slave_2' }
            steps {
                echo 'Code Review...'
                sh 'mvn pmd:pmd'
            }
        }

        stage('UnitTest') {
            agent { label 'Slave_2' }
            steps {
                echo 'Running Unit Tests...'
                sh 'mvn test'
            }
            post {
                success {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            agent any
            steps {
                echo 'Packaging...'
                sh 'mvn package'
            }
        }
    }
}
