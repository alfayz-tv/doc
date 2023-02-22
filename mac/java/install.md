# install open jdk on mac

1. download openjdk
```
look https://jdk.java.net/18/

```

2. check shum
```
shasum -a 256 openjdk-18.0.1_macos-aarch64_bin.tar.gz

```

3. export
```
 tar -xfopenjdk-18.0.1_macos-aarch64_bin.tar.gz -C $HOME/OpenJDK

```

4. setting on .zshrc
```
cat > .zshrc

export JAVA_HOME=$HOME/OpenJDK/jdk-18.0.1.jdk/Contents/Home

export PATH=$JAVA_HOME/bin:$PATH

```

5. check version
```
java -version

```