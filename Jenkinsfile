node {
    stage("checkout") {
            git branch: 'main', credentialsId: 'git-personal', url: 'https://github.com/tanovikova22/nightwatch-examples.git'

            echo "last commit:"
            sh "git log --name-status HEAD^..HEAD"
            echo BUILD_NUMBER
    }

    stage("installation") {
            nodejs('nodejs') {
                sh "npm install"
            }
    }
    stage("tests") {
            nodejs('nodejs') { 
                try {
                    sh "npm test" 
                } finally {
                    sh 'echo $WORKSPACE'
                    junit testDataPublishers: [[$class: 'JUnitFlakyTestDataPublisher']], testResults: 'tests_output/*.xml'

                    def RERUN = currentBuild.getRawBuild().actions.find { it instanceof ParametersAction }?.parameters.find {
                        it.name == 'surefire.rerunFailingTestsCount'
                    }?.value

                    echo RERUN
                }

            }
    }
}
