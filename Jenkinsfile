
pipeline {
    agent any
    
    tools {
        maven 'Maven3' // Make sure this matches your Maven name in Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls the code you pushed to GitHub earlier
                checkout scm
            }
        }

        stage('Database Check') {
            steps {
                // Verifies your Log_monitor DB is ready before deploying
                sh "sudo mysql -p'Shan@1998' Log_monitor -e 'SHOW TABLES;'"
            }
        }

        stage('Build') {
            steps {
                // Runs Maven to create the .war file
                sh 'mvn clean package'
            }
        }

        stage('Deploy to WebLogic') {
            steps {
                // Using the plugin via code bypasses the UI errors
                step([$class: 'WeblogicDeploymentPlugin',
                    taskName: 'DeployApp',
                    environment: 'LocalWebLogic', // This will refer to your manual config
                    name: 'LogMonitorApp',
                    source: 'target/LogMonitorApp.war',
                    targets: 'AdminServer'
                ])
            }
        }
    }
}

