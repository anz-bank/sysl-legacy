# Sysl

[![GitHub Actions Go-Darwin status](https://github.com/anz-bank/sysl/workflows/Go-Darwin/badge.svg)](.)
[![GitHub Actions Go-Linux status](https://github.com/anz-bank/sysl/workflows/Go-Linux/badge.svg)](.)
[![GitHub Actions node-linux status](https://github.com/anz-bank/sysl/workflows/node-linux/badge.svg)](.)
[![GitHub Actions python-darwin status](https://github.com/anz-bank/sysl/workflows/python-darwin/badge.svg)](.)
[![GitHub Actions python-linux status](https://github.com/anz-bank/sysl/workflows/python-linux/badge.svg)](.)

[![AppVeyor build](https://img.shields.io/appveyor/ci/anz-bank/sysl/master.svg?logo=appveyor)](https://ci.appveyor.com/project/anz-bank/sysl/branch/master)
[![Codecov](https://img.shields.io/codecov/c/github/anz-bank/sysl/master.svg)](https://codecov.io/gh/anz-bank/sysl/branch/master)
[![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/anz-bank/sysl)

Sysl (pronounced "sizzle") is a system specification language. Using Sysl, you
can specify systems, endpoints, endpoint behaviour, data models and data
transformations. The Sysl compiler automatically generates sequence diagrams,
integrations, and other views. It also offers a range of code generation
options, all from one common Sysl specification.

The set of outputs is open-ended and allows for your own extensions. Sysl has
been created with extensibility in mind and it will grow to support other
representations over time.

## Installation

Windows users can download the `sysl-bundle-windows.zip`, containing `sysl.exe`
and `reljam.exe`, from our
[release page](https://github.com/anz-bank/sysl/releases).

Users on other operating systems need to work with Python or Docker.

### Python

Install [Python 2.7](https://www.python.org/downloads/). If your specific
environment causes problems you might find our
[guide](docs/environment_guide.md) helpful.

Install Sysl with

```bash
> pip install sysl
```

Now you can execute Sysl on the command line with

```bash
> sysl   textpb -o out/petshop.txt /demo/petshop/petshop
> reljam model /demo/petshop/petshop PetShopModel
```

See `sysl --help` and `reljam --help` for more options.

### Docker

Install [Docker](https://docs.docker.com/install/) and pull the Docker Image
with

```bash
> docker pull anzbank/sysl
```

Consider tagging the docker image to make commands shorter

```bash
> docker tag anzbank/sysl sysl
```

Try the following commands

```bash
> docker run sysl
> docker run sysl sysl -h
> docker run sysl reljam -h
```

See `https://hub.docker.com/r/anzbank/sysl/` for more details.

## Development

Install dependencies and the `sysl` package with symlinks

```bash
> pip install pytest flake8 -e .
```

Sysl depends upon [PlantUML](http://plantuml.com/) for diagram generation. Some
of the automated tests require a PlantUML dependency. Provide PlantUML access
either via local installation or URL to remote service. Warning, for sensitive
data the public service at www.plantuml.com is not suitable. You can use one of
the following options to set up your environment:

- execute `SYSL_PLANTUML=http://www.plantuml.com/plantuml`
- add `export SYSL_PLANTUML=http://www.plantuml.com/plantuml` to your `.bashrc`
  or similar
- [install PlantUML](http://plantuml.com/starting) locally and run on port
  8080 or you can refer to the [plantuml server guide](docs/plantUML_server.md)


Test and lint the source code and your changes with

```bash
> pytest
> flake8
```

Consider using [virtualenv](https://virtualenv.pypa.io/en/stable/) and
[virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) to
isolate your development environment.

For Java tests, install [Java 8][java-8-install] and
[gradle](https://gradle.org/install/) and run

```bash
> gradle test -b test/java/build.gradle
```

If your corporate environment restricts access to `jcenter` our [environment
guide](docs/environment_guide.md) might hold the answer for you. It also
includes tips on using `virtualenv` with `gradle test`.

We encourage contributions to this project! Please have a look at the
[contributing guide](CONTRIBUTING.md) for more information.

If you need to create a release follow the [release
documentation](docs/releasing.md).

## Local Travis CI builds (experimental)

`./run-travis.sh` runs a local Travis CI build. This is intended primarily to
test Travis builds offline.

## Extending Sysl

In order to easily reuse and extend Sysl across systems, the Sysl compiler
translates Sysl files into an intermediate representation expressed as protocol
buffer messages. These protobuf messages can be consumed in your favorite
programming language and transformed to your desired output. In this way you are
creating your own Sysl exporter.

Using the [protoc compiler](https://developers.google.com/protocol-buffers/) you
can translate the definition file of the intermediate representation
`src/proto/sysl.proto` into your preferred programming language in a one-off
step or on every build. You can then easily consume Sysl models in your
programming language of choice in a typesafe way without having to write a ton
of mapping boilerplate. With that you can create your own tailored output
diagrams, source code, views, integrations or other desired outputs.

In this project, several Python based exporters exist under `src/sysl/exporters`
and the relevant Python protobuf definitions `sysl_pb2.py` have been created
from `sysl.proto` with

```bash
> protoc --python_out=src/sysl/proto  --proto_path=src/proto sysl.proto
```

If `sysl.proto` is updated, the above command needs to be re-run to update the
corresponding Python definitions in `sysl_pb2.py`.

## Status

Sysl is currently targeted at early adopters. The current focus is to improve
documentation and usability, especially error messages and warnings.


[java-8-install]: https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html
