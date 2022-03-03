pipeline {
   agent any

    stages {
        stage('Verify branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }

        stage('Docker build') {
            steps {
                pwsh(script: 'docker imaaages -a')
                pwsh(script: """
                    cd jenkins-play/
                    docker images -a
                    docker build -t jenkins-pipeline .
                    docker images -a
                    cd ..
                 """)
            }
        }
    }
}
