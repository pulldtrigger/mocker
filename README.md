# mocker
A crappy imitation of Docker, for teaching purposes

![](https://pbs.twimg.com/media/CmE8k1qVAAAZrIt.jpg)

## mocker pull

Mocker pull will download a Docker image from the Docker public repository, download the image layers and extract them into a local folder.

```
./mocker.py pull hello-world
Starting new HTTPS connection (1): auth.docker.io
Setting read timeout to None
"GET /token?service=registry.docker.io&scope=repository:library/hello-world:pull HTTP/1.1" 200 1450
Fetching manifest for hello-world:latest...
Starting new HTTPS connection (1): registry-1.docker.io
Setting read timeout to None
"GET /v2/library/hello-world/manifests/latest HTTP/1.1" 200 2750
Fetching layer sha256:c04b14da8d1441880ed3fe6106fb2cc6fa1c9661846ac0266b8a5ec8edf37b7c..
Starting new HTTPS connection (1): registry-1.docker.io
Setting read timeout to <object object at 0x7fabadba50b0>
"GET /v2/library/hello-world/blobs/sha256:c04b14da8d1441880ed3fe6106fb2cc6fa1c9661846ac0266b8a5ec8edf37b7c HTTP/1.1" 307 432
Starting new HTTPS connection (1): dseasb33srnrn.cloudfront.net
Setting read timeout to <object object at 0x7fabadba50b0>
"GET /registry-v2/docker/registry/v2/blobs/sha256/c0/c04b14da8d1441880ed3fe6106fb2cc6fa1c9661846ac0266b8a5ec8edf37b7c/data?Expires=1476758515&Signature=P~1JaRJM6u9SNOq9c8MTsie5cEN4HPixwAKza2FPGnXu85au4r0fcbUhRWFnENHyTR1ntlYajARBIbelKb4Yf92OTFyVum~hKmOs3fXz7dCTRLNQDJ6iCuGZG1apqQ7j4JJLqP8bnkIe40FZ6WbYxG3pYqv2s0lxsdsFytgvCm0_&Key-Pair-Id=APKAJECH5M7VWIS5YZ6Q HTTP/1.1" 200 974
- hello
...
Fetching layer sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4..
Starting new HTTPS connection (1): registry-1.docker.io
Setting read timeout to <object object at 0x7fabadba50b0>
"GET /v2/library/hello-world/blobs/sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4 HTTP/1.1" 307 432
Starting new HTTPS connection (1): dseasb33srnrn.cloudfront.net
Setting read timeout to <object object at 0x7fabadba50b0>
"GET /registry-v2/docker/registry/v2/blobs/sha256/a3/a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4/data?Expires=1476758516&Signature=CkRP6ohZMxL5OJ5pX9Oamsds5oP8AEafk0otQo4Udd21DA5SparSxSlJSR7JxXkF16BS8X2kdbVGxdJxehNHCsvb~Z2dIVyA9Vrr6XKgAfmgfP2prt2GixMJzi0HZDut8DRgSK57qlzvGlYRmeKL-pk5q1HCEEgwmHoTnW450NY_&Key-Pair-Id=APKAJECH5M7VWIS5YZ6Q HTTP/1.1" 200 32
...
```

## mocker image

Mocker images will show you which images you've already downloaded

```
./mocker.py images
+------------------+---------+----------+-----------------------+
| name             | version | size     | file                  |
+------------------+---------+----------+-----------------------+
| library/tomcat   | latest  | 153.9MiB | library_tomcat.json   |
| library/rabbitmq | latest  | 82.6MiB  | library_rabbitmq.json |
+------------------+---------+----------+-----------------------+
```

## mocker run

Mocker run will

- Create a veth0 bridge virtual ethernet adapter in the root namespace
- Create a network namespace for the "container"
- Bridge a veth1 adapter into the new network namespace under the IP 10.0.0.3/24
- Setup a default route for 10.0.0.1
- Create a cgroup for the processes
- ChRoot to the extracted image directory - no snapshots (yet) which is bad
- Execute the docker image's chosen Cmd, setup environment variables and write the output to /tmp/stdout

```
./mocker.py run library/tomcat
Creating cgroups sub-directories for user root
Hierarchies availables: ['hugetlb', 'perf_event', 'blkio', 'devices', 'memory', 'cpuacct', 'cpu', 'cpuset', 'freezer', 'systemd']
cgroups sub-directories created for user root
Creating cgroups sub-directories for user root
Hierarchies availables: ['hugetlb', 'perf_event', 'blkio', 'devices', 'memory', 'cpuacct', 'cpu', 'cpuset', 'freezer', 'systemd']
cgroups sub-directories created for user root
Creating cgroups sub-directories for user root
Hierarchies availables: ['hugetlb', 'perf_event', 'blkio', 'devices', 'memory', 'cpuacct', 'cpu', 'cpuset', 'freezer', 'systemd']
cgroups sub-directories created for user root
Setting ENV PATH=/usr/local/tomcat/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
Setting ENV LANG=C.UTF-8
Setting ENV JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/jre
Setting ENV JAVA_VERSION=7u111
Setting ENV JAVA_DEBIAN_VERSION=7u111-2.6.7-1~deb8u1
Setting ENV CATALINA_HOME=/usr/local/tomcat
Setting ENV TOMCAT_NATIVE_LIBDIR=/usr/local/tomcat/native-jni-lib
Setting ENV LD_LIBRARY_PATH=/usr/local/tomcat/native-jni-lib
Setting ENV OPENSSL_VERSION=1.0.2j-1
Setting ENV TOMCAT_MAJOR=8
Setting ENV TOMCAT_VERSION=8.0.38
Setting ENV TOMCAT_TGZ_URL=https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-8/v8.0.38/bin/apache-tomcat-8.0.38.tar.gz
Setting ENV TOMCAT_ASC_URL=https://www.apache.org/dist/tomcat/tomcat-8/v8.0.38/bin/apache-tomcat-8.0.38.tar.gz.asc
Finalizing
done
```

## TODO

The other 99.999% of Docker. I only had 2 days to write this!