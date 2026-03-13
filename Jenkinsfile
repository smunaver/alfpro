pipeline {

    agent any

    environment {
        DEPLOY_PATH = "/opt/alfpro"
    }

    stages {

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
                        mkdir -p ${DEPLOY_PATH}/${api}
                        cp -r publish-artifacts/${api}/* ${DEPLOY_PATH}/${api}/
                        """

                        def service = api.toLowerCase().replace(".api","-api")

                        sh """
                        sudo systemctl restart ${service}
                        """
                    }

                }

            }

        }

    }

}
