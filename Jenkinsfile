pipeline {
    agent any

    stages {

        stage('Commit') {
            steps {
                echo 'Informations sur le dernier commit :'

                sh '''
                    echo "Hash court :"
                    git log -1 --pretty=format:"%h"

                    echo ""
                    echo "Auteur :"
                    git log -1 --pretty=format:"%an"

                    echo ""
                    echo "Message :"
                    git log -1 --pretty=format:"%s"

                    echo ""
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Construction du projet...'

                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test unitaire') {
            steps {
                echo 'Exécution des tests unitaires...'

                sh 'mvn test'
            }

            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }

    post {
        success {
            echo 'SUCCESS : Pipeline terminé avec succès.'

            archiveArtifacts artifacts: 'target/*.jar',
                             fingerprint: true
        }

        failure {
            echo 'FAILURE : Le pipeline a échoué.'
        }
    }
}