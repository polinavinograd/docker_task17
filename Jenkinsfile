pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                dir('docker_task17') {
                    git (
                        url: "https://github.com/polinavinograd/docker_task17.git",
                        branch: "master",

                        changelog: true,
                        poll: true
                    )
                }
            }
        }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'a690cedc-e357-4375-81ce-8f76041a4641', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASSWORD'
                }
            }
        }
        
        stage('Docker Build Apache') {
            steps {
                dir('docker_task17/apache') {
                    script {
                        env.APACHE_TAG = generateBuildTag()
                        sh """
                            docker build -t polinavngrd/apache:${env.APACHE_TAG} .
                            docker push polinavngrd/apache:${env.APACHE_TAG}
                        """
                    }
                }
            }
        }
        
        stage('Docker Build Nginx') {
            steps {
                dir('docker_task17/nginx') {
                    script {
                        env.NGINX_TAG = generateBuildTag()
                        sh """
                            docker build -t polinavngrd/nginx:${env.NGINX_TAG} .
                            docker push polinavngrd/nginx:${env.NGINX_TAG}
                        """
                    }
                }
            }
        }
        
        stage ("Deploy on instances") {
            steps {
                dir('ansible') {
                    sh """ansible-playbook playbook.yml -e NGINX_TAG=${env.NGINX_TAG} -e APACHE_TAG=${env.APACHE_TAG}"""
                }
            }
        }
    }
}

def generateBuildTag() {
    def date = new Date()
    def formattedDate = date.format('yyyyMMdd-HHmmss')
    return "v-${formattedDate}"
}
