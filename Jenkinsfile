pipeline {
    agent any

    tools {
        maven 'Maven 3.8.8' // or your installed version in Jenkins
        jdk 'JDK 17'        // match your pom.xml Java version
    }

    environment {
        MVN_SETTINGS = "${WORKSPACE}/jenkins-settings.xml"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/rrc09/test.git', branch: 'master'
            }
        }

        stage('Deploy to CloudHub 2.0') {
            steps {
                configFileProvider([configFile(fileId: 'maven-settings', variable: 'MVN_SETTINGS')]) {
                    bat """
    mvn clean deploy -DmuleDeploy -s %MVN_SETTINGS%
"""

                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment to CloudHub 2.0 successful!"
        }
        failure {
            echo "❌ Deployment failed. Please check the logs."
        }
    }
}
