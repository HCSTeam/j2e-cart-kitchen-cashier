node {
    def dockerHome = tool name: "docker"
    env.PATH = "${dockerHome}/bin:${env.PATH}"

    stage("Checking out") {
        parallel(
                'checking scm': {
                    node {
                        checkout scm
                    }
                },
                'Pulling Docker image':  {
                    node {
                        sh 'docker pull thomasmunoz13/vup-testing'
                    }
                }
        )
    }

    stage('Unit testing') {
        withMaven(maven: 'maven'){
            sh 'mvn clean test'
        }
    }
    
    stage("Deploying to artifactory") {
        withMaven(
            maven: 'maven',
            mavenSettingsConfig: 'maven'){
            sh 'mvn -DskipTests clean package deploy:deploy'
        }
    }
}