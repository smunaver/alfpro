pipeline {

    agent any

    environment {
        SERVER = "ubuntu@192.168.0.59"
        DEPLOY_PATH = "/opt/alfpro"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/smunaver/alfpro.git'
            }
        }

        stage('Deploy APIs') {

            steps {

                script {

                    def apis = sh(
                        script: "ls publish-artifacts",
                        returnStdout: true
                    ).trim().split("\n")

                    for (api in apis) {

                        echo "Deploying ${api}"

                        sh """
                        scp -r publish-artifacts/${api}/* ${SERVER}:${DEPLOY_PATH}/${api}/
                        """

                        def service = api.toLowerCase().replace(".api","-api")

                        sh """
                        ssh ${SERVER} "sudo systemctl restart ${service}"
                        """
                    }
                }
            }
        }
    }
}
