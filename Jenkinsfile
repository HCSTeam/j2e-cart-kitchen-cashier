node {
    def dockerHome = tool name: "docker"
    env.PATH = "${dockerHome}/bin:${env.PATH}"

    stage("Checking out") {
        checkout scm
    }

    stage('Pulling Docker image') {
        sh 'docker pull thomasmunoz13/vup-testing'
    }

    stage('Unit testing') {
        withMaven(maven: 'maven') {
            sh 'mvn test'
        }
    }

    stage("Deploying to artifactory") {
        withMaven(
                maven: 'maven',
                mavenSettingsConfig: 'maven') {
            sh 'mvn clean package deploy:deploy -DskipTests'
        }
    }
}