
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
                sh "mysql -u shanmukha -p'Shan@9591' Log_monitor -e 'SHOW TABLES';"
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
        sh '''
        export DOMAIN_HOME=/home/shanmukha/Oracle/Middleware/Oracle_Home/user_projects/domains/base_domain
        source $DOMAIN_HOME/bin/setDomainEnv.sh

        java weblogic.Deployer \
        -adminurl t3://localhost:7001 \
        -username weblogic \
        -password Shan@1998 \
        -deploy \
        -name LogMonitorApp \
        -source target/LogMonitorApp.war \
        -targets MS1,MS2
        '''
    }
}

    }
}
