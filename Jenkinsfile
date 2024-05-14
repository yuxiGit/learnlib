@Library('first-lib') _

pipeline {
    agent {
        node {
            label 'ms'
            customWorkspace '/var/jenkins_home/workspace1'
        }
    }
    environment {
        ABC = 'abc'
        Test_secret = credentials('secret')
        Test_user = credentials('test01')
    }
    parameters { 
        string(name: 'lantao', defaultValue: 'lantao', description: '') 
        choice(name: 'asp', choices: ['synnex', 'zg', 'longtian'], description: '')
    }
    
    options {
        timeout(time: 10, unit: 'SECONDS')
    }
    triggers {
        cron('H H/12 * * 1-5')
    }
    tools {
        jdk 'jdk1.8'
    }
    stages {
        stage('test01') {
            steps {
                echo 'test01'
                sh "echo Secret is ${Test_secret}"
                echo "${params.lantao} is want go to ${params.asp}"
            }
        }
        stage('parallel') {
            parallel {
                stage('a') {
                    steps {
                        echo 'a'
                        script {
                            tools.PrintMes("this is sharelib", "red")
                        }    
                    }
                }
                stage('b') {
                    steps {
                        echo 'b'
                    }
                }
                stage('c') {
                    when {
                        environment name: 'lantao', value: 'lantao'
                    }
                    steps {
                        sh "echo USER is $TEST_user_USR"
                        sh 'echo "Password is $TEST_user_PSW"'
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'this is test01'
        }
        failure {
            echo 'on fail'
        }
        success {
            echo 'it is good!'
        }
    }
}
