pipeline {
    agent any
    environment {
        STAGING_SERVER = 'user@staging-server-ip'
        PROD_SERVER = 'user@production-server-ip3'
        STAGING_DIR = '/path/to/staging/dir'
        PROD_DIR = '/path/to/production/dir'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the Maven project...'
                sh ' /opt/apache-maven-3.9.6/bin/mvn clean package'
            }
            post {
                always {
                    jiraSendBuildInfo site: 'northstar-demo.atlassian.net'
                }
            }
        }
        stage('Deploy - Staging') {
            when {
                branch 'dev'
            }
            steps {
                echo 'Deploying to Staging...'
            //  sh """
            //     scp target/hello-world-1.0-SNAPSHOT.jar ${STAGING_SERVER}:${STAGING_DIR}
            // """
                
            }
            post {
                always {
                    jiraSendDeploymentInfo environmentId: 'us-stg-1', environmentName: 'dev', environmentType: 'staging'
                }
            }
        }
        stage('Deploy - Production') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to Production...'
             // sh """
             //     scp target/hello-world-1.0-SNAPSHOT.jar ${PROD_SERVER}:${PROD_DIR}
             //  """
            }
            post {
                always {
                    jiraSendDeploymentInfo environmentId: 'us-prod-1', environmentName: 'prod', environmentType: 'production'
                }
            }
        }
    }
}

