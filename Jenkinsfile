def TAG
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo '##########################################################'
                echo '###                        build                       ###'
                echo '##########################################################'

                script {
                   if (env.BRANCH_NAME == 'master') {
                       TAG = "1.0.${BUILD_NUMBER}"
                   }
                   else if (env.BRANCH_NAME == 'dev') {
                       TAG ="dev-${GIT_COMMIT}"
                   }
                   else if (env.BRANCH_NAME == 'staging') {
                       TAG ="staging-${GIT_COMMIT}"
                   }
                   echo "This is the img tag ${TAG}"
                   sh "docker build -t us.gcr.io/echo-final-project/echo:${TAG} ."
                }
            }
        }
        stage('test') {
            steps {
                echo '##########################################################'
                echo '###                        test                        ###'
                echo '##########################################################'
                echo 'no test for now'
            }
        }
        stage('publish') {
            steps {
                echo '##########################################################'
                echo '###                        publish                     ###'
                echo '##########################################################'

                sh 'gcloud auth configure-docker > /dev/null'
                sh "docker push us.gcr.io/echo-final-project/echo:${TAG}"
            }
        }
    }
    post {
        always {
            echo 'Clening Dir.'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'Succeeeded!'
        }
        unstable {
            echo 'Unstable'
        }
        failure {
            echo 'Failed'
        }
        changed {
            echo 'Changed'
        }
    }
}