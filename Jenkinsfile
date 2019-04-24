node {
    def app
    def dockerName = 'droidw'
    def dockerMayorVersion = '0'
    def dockerMinorVersion = '0'
    def dockerLoginUser = 'admin'
    def dockerLoginPassword = '1Mono2019'
    def dockerFile = 'Dockerfile'

    try {
        stage('Clone repository') {
          checkout scm
        }

        stage('Docker Login') {
          sh ("docker login -u ${dockerLoginUser} -p ${dockerLoginPassword} doomtreader.jaak-it.com:8083")
        }

        stage('Build image') {
          sh ("docker build -t ${dockerName}:${dockerMayorVersion}.${dockerMinorVersion}.${env.BUILD_NUMBER} -f ${dockerFile} .")
        }

        stage('Docker Login') {
          sh ("docker login -u ${dockerLoginUser} -p ${dockerLoginPassword} doomtreader.jaak-it.com:8083")
        }

        stage('Tag Image'){
          sh "docker tag ${dockerName}:${dockerMayorVersion}.${dockerMinorVersion}.${env.BUILD_NUMBER} doomtreader.jaak-it.com:8083/${dockerName}:${dockerMayorVersion}.${dockerMinorVersion}.${env.BUILD_NUMBER}"
        }

        stage('Push into Nexus'){
          sh  "docker push doomtreader.jaak-it.com:8083/${dockerName}:${dockerMayorVersion}.${dockerMinorVersion}.${env.BUILD_NUMBER}"
        }

        stage('Remove Unused docker image') {
            sh "docker image prune -a --filter ''until=48h'' -f"
        }


        } catch (e) {
          // If there was an exception thrown, the build failed
          throw e
        }
}
