pipeline {
    agent any

    stages {
        stage("Build & Test") {
            matrix {
                agent {
                    dockerfile {
                      //label "docker"

                        additionalBuildArgs """
                            --build-arg JAVA_VERSION=$JAVA \
                            --build-arg MAVEN_VERSION=$MAVEN \
                            --build-arg USER_UID=\$(id -u) \
                            -t mymaven:${MAVEN}-jdk-${JAVA}
                        """.stripIndent().trim()

                        args "-v /tmp/maven:/home/jenkins/.m2"
                    }
                }
                axes {
                    axis {
                        name "JAVA"
                        values "11.0.7-amzn", "8.0.282-open", "18.ea.35-open"
                    }
                    axis {
                        name "MAVEN"
                        values "3.8.4"
                    }
                }
                stages {
                    stage("Build") {
                        steps {
                            sh "mvn -version"
                            sh "mvn -DskipTests clean package"
                        }
                    }
                    stage("Test") {
                        steps {
                            sh "mvn test -P coverage"
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
