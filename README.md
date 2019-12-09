# OpenSSL w/ the FIPS Module

These are three containers (Ubuntu, CentOS, and Alpine) built with the FIPS enabled OpenSSL pacakge. The builds do have build arguments to specify custom versions of OpenSSL and FIPS. The FIPS module is only supported on the `1.0.x` branch of OpenSSL. The latest versions of OpenSSL and the FIPS module can be found on the [OpenSSL website](https://www.openssl.org/source/).

## Usage

This repo contains three `Dockerfiles` for Ubuntu, CentOS, and Alpine linux. Each is built on it's own and attempts to leave as minimal a footprint as possible after it is built. This is following the principle of reduced surface area.

### Ubuntu

```bash
docker built -t arhea/openssl-fips:2-ubuntu1604 -f Dockerfile.ubuntu1604 .
docker built -t arhea/openssl-fips:2-ubuntu1804 -f Dockerfile.ubuntu1804 .
```

### CentOS

```bash
docker built -t arhea/openssl-fips:2-centos7 -f Dockerfile.centos7 .
docker built -t arhea/openssl-fips:2-centos8 -f Dockerfile.centos8 .
```

### Alpine

```bash
docker built -t arhea/openssl-fips:2-alpine -f Dockerfile.alpine .
```
