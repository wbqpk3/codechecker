# CodeChecker server Thrift API

This directory contains the API IDL files and the generated API stubs for
CodeChecker. [Apache Thrift](https://thrift.apache.org/) is used to generate
the stubs for various programming languages (Python, JavaScript).

The Thrift compiler is executed inside a [Docker](https://www.docker.com/)
container so `docker` needs to be installed to generate the stubs.

## API change workflow:
- Modify the `.thrift` API files.
- Update Thrift API version in file: `api_version.json`
- Run `make clean` and then `make package` or `make dev_package`
in the root of the repository.