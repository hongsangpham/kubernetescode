node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build Image') {
        app = docker.build("hongsangpham23/90daysofdevops")
    }

    stage('Test Image') {
        app.inside {
            sh 'echo "Test passed"'
        }
    }

    stage('Push Image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }

    stage('Trigger ManifestUpdate') {
        echo "Triggering UpdateManifestJob"
        build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}