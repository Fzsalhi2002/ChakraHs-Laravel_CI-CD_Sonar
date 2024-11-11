pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Fzsalhi2002/ChakraHs-Laravel_CI-CD_Sonar.git', branch: 'main'
            }
        }

     
        stage('Build Laravel') {
            steps {
                bat 'php artisan serve'
            } 
        } 

        stage('Installer les dépendances Node') {
            steps {
                bat 'npm install'
            }
        }

        stage('Compiler les assets Node') {
            steps { 
                bat 'npm run build'
            } 
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') {
                        bat """
                            "${scannerHome}\\bin\\sonar-scanner.bat" ^
                            -Dsonar.projectKey=SonarQube ^
                            -Dsonar.host.url=http://localhost:9000 ^
                            -Dsonar.login=sqa_b6ada38c70dd1894c512aa16754001ada4ca5fa6 ^
                            -Dsonar.sources=./app ^
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
