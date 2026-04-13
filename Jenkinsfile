pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Select environment')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Debug') {
            steps {
                echo "BRANCH_NAME = ${env.BRANCH_NAME}"
                echo "GIT_BRANCH = ${env.GIT_BRANCH}"
                echo "ENV = ${params.ENV}"
            }
        }

        stage('Build') {
            steps {
                echo "🔨 Building project"
            }
        }

        stage('Test (Develop Only)') {
            when {
                branch 'develop'
            }
            steps {
                echo "🧪 Running tests on DEVELOP"
            }
        }

        stage('Release (Tag Only)') {
            when {
                tag "v*"
            }
            steps {
                echo "🏷 Release build triggered"
            }
        }

        stage('Deploy to PROD') {
            when {
                allOf {
                    branch 'main'
                    expression { params.ENV == 'prod' }
                }
            }
            steps {
                echo "🚀 Deploying to PRODUCTION"
            }
        }

        stage('Skip Feature Branch') {
            when {
                not {
                    expression { env.GIT_BRANCH?.contains("feature") }
                }
            }
            steps {
                echo "✅ Not a feature branch"
            }
        }

        stage('Run Main OR Develop') {
            when {
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                echo "🔀 Running on MAIN or DEVELOP"
            }
        }
    }
}
