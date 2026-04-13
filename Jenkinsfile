pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Environment')
        string(name: 'BRANCH', defaultValue: 'main', description: 'Simulate branch')
    }

    environment {
        APP_ENV = "dev"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/lakshmanvpg/jenkins-when-practice'
            }
        }

        stage('Debug') {
            steps {
                echo "GIT_BRANCH = ${env.GIT_BRANCH}"
                echo "PARAM BRANCH = ${params.BRANCH}"
                echo "ENV = ${params.ENV}"
            }
        }

        /*********** BRANCH ***********/
        stage('MAIN Branch') {
            when {
                expression { params.BRANCH == 'main' }
            }
            steps {
                echo "✅ Running on MAIN"
            }
        }

        /*********** EXPRESSION ***********/
        stage('Develop Check') {
            when {
                expression { params.BRANCH.contains('develop') }
            }
            steps {
                echo "🧠 DEVELOP branch detected"
            }
        }

        /*********** ENVIRONMENT ***********/
        stage('Environment Check') {
            when {
                environment name: 'APP_ENV', value: 'dev'
            }
            steps {
                echo "🌍 APP_ENV = DEV"
            }
        }

        /*********** EQUALS ***********/
        stage('Equals Check') {
            when {
                expression { params.ENV == 'prod' }
            }
            steps {
                echo "⚖ ENV = PROD"
            }
        }

        /*********** NOT ***********/
        stage('Not Feature Branch') {
            when {
                not {
                    expression { params.BRANCH.contains('feature') }
                }
            }
            steps {
                echo "❌ Not a feature branch"
            }
        }

        /*********** ALLOF ***********/
        stage('Main + Prod') {
            when {
                allOf {
                    expression { params.BRANCH == 'main' }
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                echo "✅ MAIN + PROD matched"
            }
        }

        /*********** ANYOF ***********/
        stage('Main OR Develop') {
            when {
                anyOf {
                    expression { params.BRANCH == 'main' }
                    expression { params.BRANCH == 'develop' }
                }
            }
            steps {
                echo "🔀 MAIN or DEVELOP"
            }
        }
    }
}
