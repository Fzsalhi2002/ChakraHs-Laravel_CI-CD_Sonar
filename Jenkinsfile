pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Fzsalhi2002/ChakraHs-Laravel_CI-CD_Sonar.git', branch: 'main'
            }
        }

        // stage('Install Deps') {
        //     steps {
        //         script {
        //             // Installer les dépendances PHP avec Composer
        //             sh 'composer install'
        //         }
        //     }
        // }
        
        // stage('Build Laravel') {
        //     steps {
        //         script {
        //             // Lancer le serveur Laravel (en arrière-plan)
        //             sh 'php artisan serve &'
        //         }
        //     }
        // }

        stage('Installer les dépendances Node') {
            steps {
                script {
                    // Installer les dépendances Node.js
                    sh 'npm install'
                }
            }
        }

        stage('Compiler les assets Node') {
            steps {
                script {
                    // Compiler les assets avec npm
                    sh 'npm run build'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Exécuter l'analyse SonarQube en utilisant le scanner
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
                    // Vérifier la qualité du code
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            echo "Analyse terminée, vérifiez SonarQube pour les résultats."
        }

        failure {
            script {
                emailext(
                    subject: "Pipeline Failed: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: """<p>Bonjour,</p>
                             <p>Le pipeline <strong>${env.JOB_NAME}</strong> a échoué à l'étape de Quality Gate lors de l'exécution de la build numéro <strong>${env.BUILD_NUMBER}</strong>.</p>
                             <p>Statut de la Quality Gate: <strong style="color: red;">${currentBuild.result}</strong></p>
                             <p>Vérifiez les détails de la build ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                             <p>Cordialement,</p>
                             <p>Votre serveur Jenkins</p>""",
                    to: 'houcine.chakra10@gmail.com', // Remplacez par les adresses souhaitées
                    from: "chakra.hs.business@gmail.com",
                    replyTo: "chakra.hs.business@gmail.com",
                    mimeType: 'text/html'
                )
            }
        }

        success {
            script {
                emailext(
                    subject: "Pipeline Succeeded: ${env.JOB_NAME} ${env.BUILD_NUMBER}",
                    body: """<p>Bonjour,</p>
                             <p>Le pipeline <strong>${env.JOB_NAME}</strong> s'est terminé avec succès à l'étape de Quality Gate lors de l'exécution de la build numéro <strong>${env.BUILD_NUMBER}</strong>.</p>
                             <p>Statut de la Quality Gate: <strong style="color: green;">${currentBuild.result}</strong></p>
                             <p>Vérifiez les détails de la build ici : <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                             <p>Cordialement,</p>
                             <p>Votre serveur Jenkins</p>""",
                    to: 'houcine.chakra10@gmail.com', // Remplacez par les adresses souhaitées
                    from: "chakra.hs.business@gmail.com",
                    replyTo: "chakra.hs.business@gmail.com",
                    mimeType: 'text/html'
                )
            }
        }
    }
}
