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
                    pushd centos7
                    mkdir hostdata
                    pushd hostdata
                    ls -l $PW1
                    kinit -V -k -t $PW1 vrigamon@REDHAT.COM
                    brew download-build ${params.brewBuildName}
                    rpm2cpio jdg-cpp-client-8.6.0.CR1-48.el7jdg.src.rpm | cpio -idmv
                    echo curl http://downloads.jboss.org/infinispan/9.3.5.Final/infinispan-server-9.3.5.Final.zip -O
                    popd
                    ls
                    ls hostadata
                    sudo docker build -t hotrod-rhel7 .
                    sudo docker run  -v $PWD/hostdata:/home/jboss/hostdata:z -t hotrod-rhel7 \
                           /bin/bash -c "echo 'Running on docker' \
                                      && ls && ls hostdata \
                                      && tar zxvf hostdata/jdg-cpp-client-8.6.0.CR1-redhat-00155-BOTH.tar.gz \
"
                """
                }
            }
        }
    }
}
