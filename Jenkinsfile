pipeline {
    options {
        // set a timeout of 60 minutes for this pipeline
        timeout(time: 60, unit: 'MINUTES')
    }
    agent {
      docker {
        #label 'master'
         image 'ubi:8.0'
      }
    }

    environment {
        DEV_PROJECT = "anand-pavithran-dev"
        STAGE_PROJECT = "anand-pavithran-stage"
        APP_GIT_URL = "https://github.com/anand-pavithran/mytest2"
        APP_NAME = "myweb"
    }


    stages {

        stage('Package install') {
            steps {
                echo '### Installing Packages ###'
                sh '''
                        yum install httpd -y
                   '''
            }
        }

        stage('Create an WebPage') {
            steps {
                echo '### Creating Webpage ###'
                sh 'echo "Demo of Jenkins Pipeline" >> /var/www/html/index.html'
            }
        }


        stage('Launch new app in DEV env') {
            steps {
                echo '### Cleaning existing resources in DEV env ###'
                sh '''
                        oc project ${DEV_PROJECT}
                        oc delete all -l app=${APP_NAME}
                        sleep 5
                   '''

                echo '### Creating a new app in DEV env ###'
                sh '''
                        oc project ${DEV_PROJECT}
                        oc new-app --name ${APP_NAME} ${APP_GIT_URL} 
                        oc expose svc/${APP_NAME} 
                   '''
            }
        }
}
