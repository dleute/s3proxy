S3Proxy
=======
S3Proxy allows applications using the
[S3 API](https://en.wikipedia.org/wiki/Amazon_S3#S3_API_and_competing_services)
to access other storage backends,
e.g., local file system, Google Cloud Storage, Microsoft Azure, OpenStack Swift.

Installation
------------
Users can [download releases](https://github.com/andrewgaul/s3proxy/releases)
from GitHub.  One can also build the project by running `mvn package` which
produces a binary at `target/s3proxy`.  S3Proxy requires Java 7 to run.

Examples
--------
Linux and Mac OS X users can run S3Proxy via the executable jar:

```
chmod +x s3proxy
s3proxy --properties s3proxy.conf
```

Windows users must explicitly invoke java:

```
java -jar s3proxy --properties s3proxy.conf
```

Users can configure S3Proxy via a properties file.  An example using Rackspace
CloudFiles (based on OpenStack Swift) as the backing store:

```
s3proxy.endpoint=http://127.0.0.1:8080
s3proxy.authorization=aws-v2
s3proxy.identity=local-identity
s3proxy.credential=local-credential
jclouds.provider=rackspace-cloudfiles-us
jclouds.identity=remote-identity
jclouds.credential=remote-credential
```

Another example using the local file system as the backing store with anonymous
access:

```
s3proxy.authorization=none
s3proxy.endpoint=http://127.0.0.1:8080
jclouds.provider=filesystem
jclouds.identity=identity
jclouds.credential=credential
jclouds.filesystem.basedir=/tmp
```

S3Proxy can listen on HTTPS by setting the `secure-endpoint` and
[configuring a keystore](http://wiki.eclipse.org/Jetty/Howto/Configure_SSL#Generating_Keys_and_Certificates_with_JDK_keytool).
An example:

```
s3proxy.secure-endpoint=https://127.0.0.1:8080
s3proxy.keystore-path=keystore.jks
s3proxy.keystore-password=password
```

To setup the keystore, do

```
$ keytool -keystore keystore.jks -alias aws -genkey -keyalg RSA
```

Use `*.s3.amazonaws.com` as the `CN` if you wish to proxy
access to Amazon S3 itself. Applications will reject the self-signed
certificate, unless you import it to the application's trusted
store. If the application is written in Java, you can do:

```
$ keytool -exportcert -keystore keystore.jks -alias aws -rfc > aws.crt
$ keytool -keystore $JAVA_HOME/jre/lib/security/cacerts -import -alias aws -file aws.crt -trustcacerts
```

Users can also set other Java,
[jclouds](https://github.com/jclouds/jclouds/blob/master/core/src/main/java/org/jclouds/Constants.java),
and [S3Proxy](https://github.com/andrewgaul/s3proxy/blob/master/src/main/java/org/gaul/s3proxy/S3ProxyConstants.java)
properties.

Supported storage backends
--------------------------
* atmos
* aws-s3
* azureblob
* filesystem (on-disk storage)
* google-cloud-storage
* openstack-swift
* rackspace-cloudfiles-uk and rackspace-cloudfiles-us
* s3
* transient (in-memory storage)

Limitations
-----------
S3Proxy does not support:

* AWS signature V4, see [#24](https://github.com/andrewgaul/s3proxy/issues/24)
* POST upload policies, see [#73](https://github.com/andrewgaul/s3proxy/issues/73)
* object metadata with filesystem provider on Mac OS X, see [JDK-8030048](https://bugs.openjdk.java.net/browse/JDK-8030048)
* object server-side encryption
* object versioning, see [#74](https://github.com/andrewgaul/s3proxy/issues/74)
* XML ACLs

S3Proxy emulates the following operations:

* copy multi-part objects, see [#76](https://github.com/andrewgaul/s3proxy/issues/76)

References
----------

* Apache [jclouds](http://jclouds.apache.org/) provides object store support for S3Proxy
* Ceph [s3-tests](https://github.com/ceph/s3-tests) help maintain and improve compatibility with the S3 API
* [fake-s3](https://github.com/jubos/fake-s3) and [S3 ninja](https://github.com/scireum/s3ninja) provide functionality similar to S3Proxy when using the filesystem provider
* Another project named [s3proxy](https://github.com/abustany/s3proxy) provides HTTP access to non-S3-aware applications
* [SwiftProxy](https://github.com/bouncestorage/swiftproxy) provides similar functionality for the OpenStack Swift API

License
-------
Copyright (C) 2014-2015 Andrew Gaul

Licensed under the Apache License, Version 2.0
