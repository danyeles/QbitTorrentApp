pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'qbittorrentofficial/qbittorrent-nox:latest'
        CONTAINER_NAME = 'qbittorrent-nox'
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

        stage('Deploy Docker Image') {
            steps {
                script {
                    sh """
                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        --restart always \
                        -e QBT_EULA=accept \
                        -e QBT_VERSION=latest \
                        -e QBT_WEBUI_PORT=8787 \
                        -e TZ=America/Monterrey \
                        -p 6881:6881/tcp \
                        -p 6881:6881/udp \
                        -p 8787:8787/tcp \
                        --read-only \
                        --stop-timeout 1800 \
                        --tmpfs /tmp \
                        -v ${CONFIG_PATH}:/config \
                        -v /media/Media:/Media \
                        -v /media/Media/Downloads:/downloads \
                        -v /media/Media/Movies:/Movies \
                        -v /media/Media/MyMovies:/MyMovies \
                        -v /media/Media/TVShows:/TVShows \
                        -v /media/Media/MyTVShows:/MyTVShows \
                        ${DOCKER_IMAGE}
                    """
                }
            }
        }
        stage('Get Temporary password') {
            steps {
                script {
                    sh """
                    docker logs qbittorrent-nox
                    """
                }
            }
        }
    }
}
