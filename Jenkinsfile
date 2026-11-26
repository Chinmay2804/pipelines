pipeline {
    agent any

    tools {
        maven 'mymaven'
    }

    stages {
        stage('checkout code') {
            steps {
                git 'https://github.com/Sonal0409/DevOpsCodeDemo.git'
            }
        }
        stage('Code review') {
            steps {
                sh 'mvn pmd:pmd'
            }
            post {
                always {
                    echo 'Code review complete'
                }
                success {
                    echo 'Code review success'
                    echo 'Generating pmd trend report'
                    recordIssues sourceCodeRetention: 'LAST_BUILD', tools: [pmdParser(pattern: '**/pmd.xml')]
                }
                failure {
                    echo 'Code review fail'
                }
            }
        }
        stage('Compile Code') {
            steps {
                sh 'mvn compile'
            }
        }
        stage ('Test Code') {
            steps {
                sh 'mvn test'
            }
            post {
                success {
                    echo 'Test success'
                    recordIssues sourceCodeRetention: 'LAST_BUILD', tools: [junitParser(pattern: '**/target/surefire-reports/*.xml')]
                }
            }
        }
        stage ('Package code') {
            steps {
                sh 'mvn package'
            }
        }
    }
}
