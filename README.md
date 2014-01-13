Instructions

* install GVM, the command-line tool for installing Groovy-friendly projects :-) http://gvmtool.net/

```
curl -s get.gvmtool.net | bash
````

* lazybones is a template project generator/installer that way you can create a Ratpack app easily

```
gvm install lazybones
mkdir ratpacktest
cd ratpacktest
```

* create a demo ratpack app

```
lazybones create ratpack .
```

* give Gradle a bit more memory to avoid a Java Heap Space error later on

```
export GRADLE_OPTS=-Xmx1024m
```

* if we use the fatJar approach, which jars everything up (even static web resources, etc) then there's already a proper META-INF/MANIFEST.MF file with the Main-Class and Class-Path attributes correctly specified

```
./gradlew fatJar
````

* let's setup the cf stuff

```
cf target api.run.pivotal.io
cf login --email myemail --password mypassword
cf switch-space development
```

# create a manifest.yml file like this
# (in particular for specifying the port, as otherwise Ratpack uses 5050)

```
---
applications:
- name: ratpacktest
  memory: 512M
  instances: 1
  host: ratpacktest
  domain: cfapps.io
  path: ./build/libs/ratpacktest-fat.jar
  env:
    ratpack-port: 80
```
