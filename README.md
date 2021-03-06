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

Usage
-----
Configure S3Proxy via a properties file.  An example using the local
file system as the storage backend with anonymous access:

```
s3proxy.authorization=none
s3proxy.endpoint=http://127.0.0.1:8080
jclouds.provider=filesystem
jclouds.identity=identity
jclouds.credential=credential
jclouds.filesystem.basedir=/tmp/s3proxy
```

First create the filesystem basedir:

```
mkdir /tmp/s3proxy
```

Next run S3Proxy.  Linux and Mac OS X users can run the executable jar:

```
chmod +x s3proxy
s3proxy --properties s3proxy.conf
```

Windows users must explicitly invoke java:

```
java -jar s3proxy --properties s3proxy.conf
```

Finally test by creating a bucket then listing all the buckets:

```
$ curl --request PUT http://localhost:8080/testbucket

$ curl http://localhost:8080/
<?xml version="1.0" ?><ListAllMyBucketsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Owner><ID>75aa57f09aa0c8caeab4f8c24e99d10f8e7faeebf76c078efc7c6caea54ba06a</ID><DisplayName>CustomersName@amazon.com</DisplayName></Owner><Buckets><Bucket><Name>testbucket</Name><CreationDate>2015-08-05T22:16:24.000Z</CreationDate></Bucket></Buckets></ListAllMyBucketsResult>
```

See the wiki for [examples of other providers](https://github.com/andrewgaul/s3proxy/wiki).

Supported storage backends
--------------------------
* atmos
* aws-s3
* azureblob
* filesystem (on-disk storage)
* google-cloud-storage
* hpcloud-objectstorage
* openstack-swift
* rackspace-cloudfiles-uk and rackspace-cloudfiles-us
* s3
* swift and swift-keystone (legacy)
* transient (in-memory storage)

Limitations
-----------
S3Proxy does not support:

* AWS signature V4, see [#24](https://github.com/andrewgaul/s3proxy/issues/24)
* POST uploads, see [#73](https://github.com/andrewgaul/s3proxy/issues/73)
* object server-side encryption
* object versioning, see [#74](https://github.com/andrewgaul/s3proxy/issues/74)
* XML ACLs

S3Proxy emulates the following operations:

* multi-part uploads, see [#2](https://github.com/andrewgaul/s3proxy/issues/2)
* copy objects, see [#46](https://github.com/andrewgaul/s3proxy/issues/46)

The wiki collects other [compatability notes](https://github.com/andrewgaul/s3proxy/wiki/Compatibility-notes).

References
----------

* Apache [jclouds](http://jclouds.apache.org/) provides object store support for S3Proxy
* Ceph [s3-tests](https://github.com/ceph/s3-tests) help maintain and improve compatibility with the S3 API
* [fake-s3](https://github.com/jubos/fake-s3) and [S3 ninja](https://github.com/scireum/s3ninja) provide functionality similar to S3Proxy when using the filesystem provider
* Another project named [s3proxy](https://github.com/abustany/s3proxy) provides HTTP access to non-S3-aware applications
* [GlacierProxy](https://github.com/bouncestorage/glacier-proxy) and [SwiftProxy](https://github.com/bouncestorage/swiftproxy) provide similar functionality for the Amazon Glacier and OpenStack Swift APIs
* [s3proxydocker](https://github.com/ritazh/s3proxydocker) packages S3Proxy as a Docker container
* [swift3](https://github.com/openstack/swift3) provides an S3 middleware for OpenStack Swift

License
-------
Copyright (C) 2014-2015 Andrew Gaul

Licensed under the Apache License, Version 2.0
