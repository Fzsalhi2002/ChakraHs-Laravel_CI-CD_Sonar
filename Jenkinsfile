pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Fzsalhi2002/ChakraHs-Laravel_CI-CD_Sonar.git', branch: 'main'
            }
        }

        stage('Install Deps') {
            steps {
                echo 'composer install'
            }
        }
        
        stage('Build Laravel') {
            steps {
                echo 'php artisan serve &'
            } 
        } 

        stage('installer les dépendances Node') {
            steps {
                echo 'npm install'
            }
        }

        stage('compiler les assets Node') {
            steps { 
                echo 'npm run build'
            } 
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=SonarQube \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=sqa_b6ada38c70dd1894c512aa16754001ada4ca5fa6 \
                            -Dsonar.sources=./app \
                            -Dsonar.exclusions="vendor/,storage/,bootstrap/cache/"
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            echo "Analyse terminée, vérifiez SonarQube pour les résultats."
        }
    }
}
