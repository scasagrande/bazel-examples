# building rust crate rdkafka with bazel

In this example we:

- Set up a basic rust project using crate [rdkafka](https://crates.io/crates/rdkafka)
- Define a bazel-native build of [librdkafka](https://github.com/confluentinc/librdkafka)
- Use crate annotations to correctly build crate [rdkafka-sys](https://crates.io/crates/rdkafka-sys)

## Pre-reqs

You should have a modern version of bazelisk installed.

To run the tests you will need a functional instance of kafka available on port 9092, or something that can start containers based on a compose.yaml file.

## Running

The tests in this example should work on Linux and macOS hosts.

The image will be automatically cross-compiled for Linux, targeting your system's native architecture.

It is suggested that you use the included compose.yaml file in order to stand-up a known-good kafka deployment for these tests.

For example, with docker compose:

```shell
docker compose up -d
```

Then to run the tests:

```shell
bazel test //...
```

## References

- [Brian Myers in the Bazel slack](https://bazelbuild.slack.com/team/U023FM87D42) for providing a template on how to build librdkafka with Bazel
- [Federico Giraud](https://github.com/fede1024) for writing rust-rdkafka and providing tests
