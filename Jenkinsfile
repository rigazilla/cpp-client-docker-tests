pipeline {
    agent none
    stages{
        stage('Test Linux') {
            agent {label 'RPM' }
            environment {
                relName = "${params.relMaj}.${params.relMin}.${params.relMic}.${params.relLab}"
                relBuild = sh returnStdout: true, script: 'printf "%05d" ${BUILD_NUMBER}'
            }
            steps {
                git url: 'https://github.com/rigazilla/cpp-client-docker-tests.git'
                sh "echo Testing build ${brewBuildName}"
                                // Cloning jdg-cpp-client repo via rhpkg
                withCredentials([file(credentialsId: 'vrigamon-krb', variable: 'PW1')]) {
                sh """
                    mkdir hostdata
                    pushd hostdata
                    ls -l $PW1
                    kinit -V -k -t $PW1 vrigamon@REDHAT.COM
                    brew download-build ${params.brewBuildName}
                    curl http://downloads.jboss.org/infinispan/9.3.5.Final/infinispan-server-9.3.5.Final.zip -O
                    popd
                    pushd centos7
                    sudo docker run  -v $PWD/../hostdata:/home/jboss/hostdata:z -t hotrod-rhel7 echo "run command"
                """
                }
            }
        }
    }
}
