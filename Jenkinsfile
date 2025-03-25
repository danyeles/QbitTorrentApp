pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'qbittorrentofficial/qbittorrent-nox:latest'
        CONTAINER_NAME = 'qbittorrent-nox'
        USERNAME = params.LOCAL_USER  // Use the parameter here
        CONFIG_PATH = '/home/docker/qbittorrentofficial/config'
    }

    stages {
        stage('Pull Docker Image') {
            steps {
                script {
                    sh "docker pull ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh """
                        docker run -d \\
                        --name ${CONTAINER_NAME} \\
                        --restart always \\
                        -e QBT_EULA=accept \\
                        -e QBT_VERSION=latest \\
                        -e QBT_WEBUI_PORT=8787 \\
                        -e TZ=America/Monterrey \\
                        -p 6881:6881/tcp \\
                        -p 6881:6881/udp \\
                        -p 8787:8787/tcp \\
                        --read-only \\
                        --stop-timeout 1800 \\
                        --tmpfs /tmp \\
                        -v ./config:/config \\
                        -v ${CONFIG_PATH}:/config \\
                        -v /media/${USERNAME}/Media:/Media \\
                        -v /media/${USERNAME}/Media/Downloads:/downloads \\
                        -v /media/${USERNAME}/Media/Movies:/Movies \\
                        -v /media/${USERNAME}/Media/MyMovies:/MyMovies \\
                        -v /media/${USERNAME}/Media/TVShows:/TVShows \\                        
                        -v /media/${USERNAME}/Media/MyTVShows:/MyTVShows \\
                        ${DOCKER_IMAGE}
                    """
                }
            }
        }

        stage('Post Steps') {
            steps {
                echo 'Container is up and running.'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        cleanup {
            script {
                sh "docker stop ${CONTAINER_NAME} || true"
                sh "docker rm ${CONTAINER_NAME} || true"
            }
        }
    }
}
