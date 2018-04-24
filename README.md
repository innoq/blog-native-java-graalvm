# blog-native-java-graalvm

### Setup

```bash
wget https://github.com/oracle/graal/releases/download/vm-1.0.0-rc1/graalvm-ce-1.0.0-rc1-linux-amd64.tar.gz
docker build . -t graalvm
```

### Hello World
```bash
docker run --rm -tiv `pwd`:/work -w /work
bash-4.2# javac HelloWorld.java
bash-4.2# native-image HelloWorld
bash-4.2# ./helloworld
```

### Hello World Server
```bash
docker run --rm -tiv `pwd`:/work -w /work
bash-4.2# javac HelloWorldServer.java
bash-4.2# native-image HelloWorldServer
bash-4.2# ./helloworldserver
```

### Containerize
```bash
docker build -f=Dockerfile.full . -t hello-graalvm
```