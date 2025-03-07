# Using rustls-openssl and enabling FIPS mode

In this example our goal is as follows:

- Build a rustls application
- Use openssl as our crypto provider
- Bundle our application into an OCI image
- Start our application with FIPS mode enabled

To get there, we will use the following:

- [rustls-openssl](https://github.com/tofay/rustls-openssl) to use openssl as our crypto provider, instead of [aws-ls-rs](https://github.com/aws/aws-lc-rs). [The latter can be FIPS-140-3 compliant](https://github.com/aws/aws-lc/blob/main/crypto/fipsmodule/FIPS.md), but in this case we explicitly want to use openssl.
- rules_rust crate annotations in order to correctly build crate [openssl-sys](https://github.com/sfackler/rust-openssl) under Bazel. This can be useful for anyone looking to depend on the openssl rust crate. See [third_party/BUILD.bazel](./third_party/BUILD.bazel) for details.
- Add our binary to a [Red Hat Universal Base Image 8 Minimal](https://catalog.redhat.com/software/containers/ubi8/ubi-minimal) image, which will contain a FIPS-140-3 compliant copy of openssl.
- This example will use openssl 1.1.1, but the process is the same for openssl 3.x. See [scasagrande/openssl-prebuilt](https://github.com/scasagrande/openssl-prebuilt) for my quick example implementation on how to use a pre-built copy of openssl for this.
- container_structure_test to run our image and make sure that all tests report back as passing from inside the container.

## Pre-reqs

You should have a modern version of bazelisk installed, and either podman or docker installed and available.

## Running

This example should work on Linux and macOS hosts.

The image will be automatically cross-compiled for Linux, targeting your system's native architecture.

Small sleeps have been added in the tests before enabling FIPS mode. This helps to prevent a failure condition where we load the image and enable FIPS mode too quickly, resulting in failures. I presume that this is due to mitigations due to potential attacks with the system not yet having enough entropy.

```shell
bazel test //:structure_test
```

## Warning!

Please note that I am not a crypto/openssl/FIPS expert, and you should not rely on this example as proof of full compliance. You should do your own testing and auditing!

## Note

Developer note for updating rust dependencies:

```shell
bazel run //third_party:vendor_rust_deps
bazel run //third_party:patch_deps -- $(bazel info workspace)/third_party/crates/defs.bzl
bazel mod tidy
```
