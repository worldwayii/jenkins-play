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
                sh(script: 'docker images -a')
                sh(script: """
                    cd azure-vote/
                    docker images -a
                    docker build -t jenkins-pipeline .
                    docker images -a
                    cd ..
                """)
            }
        }
        stage('Start test app') {
            steps {
                sh(script: """
                  docker-compose up -d

                  ./scripts/test_container.sh
                """)
            }
           
            post {
                success{
                    echo "Test started running"
                }
                failure {
                    echo "Test app failed"
                }
            }
            
        }
        stage('Run Tests'){
            steps {
                sh(script: """
                  pytest .tests/test_sample.py
                """)
            }
        }
        stage('Stop Test app') {
            steps {
                sh(script: """
                  docker-compose down
                """)
            }
        }
      
    }
}
