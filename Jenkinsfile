node {
    def app
    stage('Clone repository') {
        checkout scm
    }
    stage('Build image') {
        if (env.BRANCH_NAME == 'dev') {
            app = docker.build("netkovjordan/jenkins-project")
        } else {
            echo "Skipping build and push for branch ${env.BRANCH_NAME}"
        }
    }
    stage('Push image') {
        if (env.BRANCH_NAME == 'dev') {
            docker.withRegistry('https://registry.hub.docker.com', 'DockerHub') {
                app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
                app.push("${env.BRANCH_NAME}-latest")
                // signal the orchestrator that there is a new version
            }
        } else {
            echo "Skipping build and push for branch ${env.BRANCH_NAME}"
        }
    }
}
