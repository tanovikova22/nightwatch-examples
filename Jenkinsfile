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
                } catch(err) {
                    echo 'retring failed tests'
                    sh 'npm test --retries 2'
                }finally {
                    sh 'echo $WORKSPACE'
                    junit testDataPublishers: [[$class: 'JUnitFlakyTestDataPublisher']], testResults: 'tests_output/*.xml', skipPublishingChecks: true
                }

            }
    }
}
