Instructions
===============

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

* potentially give Gradle a bit more memory to avoid a Java Heap Space error later on

```
export GRADLE_OPTS=-Xmx1024m
```

* use the distZip task to create the archive to be uploaded on CloudFoundry

```
./gradlew distZip
````

* let's setup the CloudFoundry access (we assumed you have installed the cf command-line tooling with `gem install cf`)

```
cf target api.run.pivotal.io
cf login --email myemail --password mypassword
cf switch-space development
```

* create a manifest.yml file as follows, in particular, notice the usage of the Java buildpack that Ben Hale developed with specific Ratpack support that will soon be deployed on cfapps.io:

```
---
applications:
- name: ratpacktest
  memory: 512M
  instances: 1
  host: ratpacktest
  domain: cfapps.io
  path: build/distributions/ratpacktest.zip
  buildpack: https://github.com/cloudfoundry/java-buildpack.git

```

* and then, you should be able to deploy your Ratpack application to CloudFoundry with:

```
cf push
```

