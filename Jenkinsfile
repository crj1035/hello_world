pipeline {
    agent any
    tools {
        maven 'maven 3.9.4'
    }
    stages {
        stage('Checkout')
        {
            steps {
                // modifications des credentials et du git
                git branch: 'mainf', credentialsId: '558f2284-0ee7-44db-bfa7-68d8b433370e', url: 'git@github.com:crj1035/hello_world.git'
            }
        }
        stage('Generate')
        {
            steps {
                sh 'mvn generate-test-resources process-test-resources'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true -DtestFailureIgnore=true test'
            }
        }
        stage('Generate Jar') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true -DtestFailureIgnore=true package'
            }
        }
        stage('Process build') {
               parallel {
                    stage('Artifact Jar') {
                        steps {
                            archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                        }
                    }
                    stage('Process Test reports') {
                        steps {
                            junit checksName: 'Jenkins Junit Tests', skipMarkingBuildUnstable: true, testResults: 'target/surefire-reports/*.xml'
                        }
                    }
               }
        }
    }
}
