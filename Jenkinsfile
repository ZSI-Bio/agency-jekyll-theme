pipeline {
    agent any
       stages {
            stage('Build page') {
                steps {
                  echo "Building page"
                  sh "docker pull jekyll/jekyll"
                  sh 'export JEKYLL_VERSION=3.5 && docker run --rm --volume=$(pwd | sed "s|/var/jenkins_home|/data/home/jenkins|g"):/srv/jekyll -i jekyll/jekyll:$JEKYLL_VERSION jekyll build '
                  sh 'sudo chown -R zsibio-jenkins *'
                }

                }
            stage('Rebuild Docker container') {
                    steps {
                      echo "Building a fresh Docker container"
                      sh "docker build -t zsi-bio/zsi-bio-page ."
                    }

            }
            stage('Start Docker container') {
                    steps {
                      echo "Starting a fresh Docker container"
                      sh 'if [ $(docker ps | grep zsi-bio-page | wc -l) -gt 0 ]; then docker stop zsi-bio-page && docker rm zsi-bio-page; fi'
                      sh "docker run -d --name zsi-bio-page zsi-bio/zsi-bio-page"
                    }

            }

      }
}
