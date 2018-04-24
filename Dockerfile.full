FROM oraclelinux:7-slim AS BASE

ENV GRAALVM_PKG=graalvm-ce-1.0.0-rc1-linux-amd64.tar.gz \
    JAVA_HOME=/usr/graalvm-1.0.0-rc1/jdk \
    PATH=/usr/graalvm-1.0.0-rc1/bin:$PATH

ADD $GRAALVM_PKG /usr/

RUN yum -y install gcc; \
    yum -y install zlib-devel;  \
    alternatives --install /usr/bin/java  java  $JAVA_HOME/bin/java  20000 && \
    alternatives --install /usr/bin/javac javac $JAVA_HOME/bin/javac 20000 && \
    alternatives --install /usr/bin/jar   jar   $JAVA_HOME/bin/jar   20000

# building
COPY *.java /target/
WORKDIR /target
RUN javac *.java
RUN native-image HelloWorldServer
RUN native-image HelloWorld

# packing minimal container
FROM scratch

COPY --from=BASE /lib64/libc.so.6 /lib64/libc.so.6
COPY --from=BASE /lib64/libdl.so.2 /lib64/libdl.so.2
COPY --from=BASE /lib64/libpthread.so.0 /lib64/libpthread.so.0
COPY --from=BASE /lib64/libz.so.1 /lib64/libz.so.1
COPY --from=BASE /lib64/librt.so.1 /lib64/librt.so.1
COPY --from=BASE /lib64/libcrypt.so.1 /lib64/libcrypt.so.1
COPY --from=BASE /lib64/ld-linux-x86-64.so.2 /lib64/ld-linux-x86-64.so.2
COPY --from=BASE /lib64/libfreebl3.so /lib64/libfreebl3.so

COPY --from=BASE /target/helloworld /
COPY --from=BASE /target/helloworldserver /

CMD ["/helloworld"]