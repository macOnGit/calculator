pipeline {
    agent any
    // triggers {
    //     pollSCM('H/5 * * * *')
    // }
    // options {
    //     buildDiscard logRotator(
    //         artifactDaysToKeepStr: '',
    //         artifactNumToKeepStr: '5',
    //         daysToKeepStr: '',
    //         numToKeepStr: '5')
    // }
    stages {
        stage('Compile') {
            steps {
                sh './gradlew compileJava'
            }
        }
        stage('Unit test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Code coverage') {
            steps {
                sh './gradlew test jacocoTestReport'
                publishHTML(target: [
                    reportDir: 'build/reports/jacoco/test/html',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Report'
                ])
                sh './gradlew test jacocoTestCoverageVerification'
            }
        }
        stage('Static code analysis') {
            steps {
                sh './gradlew checkstyleMain'
                publishHTML(target: [
                    reportDir: 'build/reports/checkstyle',
                    reportFiles: 'main.html',
                    reportName: 'Checkstyle Report'
                ])
            }
        }
        stage('Package') {
            steps {
                sh "./gradlew build"
            }
        }
        stage("Docker build") {
            steps {
                def myEnv = docker.build 'cmacondocker/calculator'
                // sh "docker build -t cmacondocker/calculator ."
            }
        }
    }
}