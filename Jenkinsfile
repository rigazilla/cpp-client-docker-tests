pipeline {
    agent none
    stages{
        stage('Test Linux') {
            agent {label 'RPM' }
            steps {
                git url: 'https://github.com/rigazilla/cpp-client-docker-tests.git'
                sh "echo Testing build ${brewBuildName}"
                                // Cloning jdg-cpp-client repo via rhpkg
                withCredentials([file(credentialsId: 'vrigamon-krb', variable: 'PW1')]) {
                sh """
                    pushd ${params.distro}
                    rm -rf hostdata
                    mkdir hostdata
                    pushd hostdata
                    ls -l $PW1
                    kinit -V -k -t $PW1 vrigamon@REDHAT.COM
                    brew download-build ${params.brewBuildName}
                    rpm2cpio ${params.brewBuildName} | cpio -idmv
                    curl http://downloads.jboss.org/infinispan/9.4.1.Final/infinispan-server-9.4.1.Final.zip -O
                    popd
                    sudo docker build -t ${params.distro} .
                    sudo docker run  -v `pwd`/hostdata:/home/jboss/hostdata:z -t ${params.distro} \
                           /bin/bash -c 'echo Running on docker \
                                      && ls && ls hostdata \
                                      && tar zxvf hostdata/jdg-cpp-client-8.6.0.CR1-redhat-00155-BOTH.tar.gz \
                                      && unzip hostdata/infinispan-server-9.4.1.Final.zip \
                                      && export JAVA_HOME=/usr/lib/jvm/java-1.8.0 \
                                      && export JBOSS_HOME=/home/jboss/infinispan-server-9.4.1.Final/ \
                                      && ( cp ~/jdg-cpp-client-8.6.0.CR1-redhat-00155-BOTH/${params.srcDir}/test/data/* \$JBOSS_HOME/standalone/configuration || true ) \
                                      && cd jdg-cpp-client-8.6.0.CR1-redhat-00155-BOTH/ \
                                      && cd ${params.srcDir}/ \
                                      && rm -rf build \
                                      && mkdir build \
                                      && cd build \
                                      && cmake -DINSTALL_GTEST=FALSE .. \
                                      && cmake --build . \
                                      && ctest -V \
'
                """
                }
            }
        }
    }
}
