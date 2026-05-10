# Building

This guide provides detailed instructions to build the project, namely how to enable optional code quality tools.

## Table of contents

- [Requirements](#requirements)
    - [Using Docker](#using-docker)
- [CMake Presets](#cmake-presets)
- [CMake coding style and format](#cmake-coding-style-and-format)

## Requirements

These tools are required to configure and build the project:

- CMake >= 3.28.
- Git.

The following are the code quality tools used by the project (only required for developers contributing to the project or working on internal tooling):

- cmake-format: CMake code formatting.
- cmake-lint: CMake code linting.

### Using Docker

There is a Docker image available in this project that contains all the dependencies to build the project, as well as the development tools. This allows to quickly use the project without installing any tool in the local machine.

The instructions to use the docker image can be found [here](./docker/README.md).

## CMake Presets

This project supports CMake Presets ([CMakePresets.json](./CMakePresets.json)), which specifies some common configuration options to facilitate the building of the project and the sharing of these settings with the developers/users.

To list all the CMake configuration presets available for this project:

```sh
$ cmake --list-presets=configure
$ cmake --list-presets=build
$ cmake --list-presets=test
```

## CMake coding style and format

This projects follows my [CMake coding style guide](https://github.com/hugorbarbosa/cmake-style-guide), and to ensure consistency, cmake-format and cmake-lint are used to format and check the CMake code.

Using the respective CMake Preset for cmake-format:

```sh
$ cmake --preset cmake-format
$ # To just check the files without modifying them.
$ cmake --build --preset cmake-format-check
$ # To format the files.
$ cmake --build --preset cmake-format-apply
```

These targets use cmake-format to verify/apply the desired format of the CMake code, and create a report file in the respective build directory, named as `cmake_format_report.log`.

Regarding cmake-lint, using the respective CMake Preset:

```sh
$ cmake --preset cmake-lint
$ cmake --build --preset cmake-lint
```

The target uses cmake-lint to verify the desired lint options of the CMake code, and creates a report file in the respective build directory, named as `cmake_lint_report.log`.

The builds for the cmake-format and cmake-lint targets succeed only if the CMake files are formatted accordingly to the [configuration](.cmake-format.py) file. The CMake files to be verified are configured through CMake.
