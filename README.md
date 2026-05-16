# CMake modules

This project provides a collection of reusable CMake modules, designed to help you quickly get started and setup the software development workflows of a project.

## Table of contents

- [Features](#features)
- [Integration](#integration)
- [Usage](#usage)
- [Building](#building)
- [License](#license)

## Features

This project contains various CMake modules that I have created during the development of my software projects. The following are the features provided by this project:

- CMake modules that can be quickly integrated in a software project that uses code quality tools for:
    - Code coverage.
    - Source code formatting and linting (for C/C++ and CMake code).
    - Static analysis.
    - Sanitizers.
    - Generation of documentation.
- Code quality tools configured to be easily executed and ready for integration into CI/CD pipelines.

## Integration

There are many ways to make the CMake modules available in your project.

### Using CMake `FetchContent` feature

CMake `FetchContent` feature can be used to automatically download this project as a dependency when configuring your project. For that, you just need to place this code in your `CMakeLists.txt` file:

```cmake
include(FetchContent)
FetchContent_Declare(
    cmake_modules
    GIT_REPOSITORY https://github.com/hugorbarbosa/cmake-modules
    GIT_TAG <tag> # Tag, branch or commit hash.
)
FetchContent_MakeAvailable(cmake_modules)
```

### Using as `git submodule`

You can use this project as a `git submodule` in your project. For this case, you need to configure the submodule as follows (the example places the project in the `external` directory inside of your project):

```sh
git submodule add https://github.com/hugorbarbosa/cmake-modules external/cmake-modules
```

With the previous command, a `.gitmodules` file is created and the CMake modules are available in your project. You can select a specific tag, branch or commit hash of the CMake modules project that you want to use from its directory.

With this option, you need to update the submodule for setting up your project:

```sh
git submodule update --init --recursive
```

### Copying the entire project

You can also copy the entire project source tree into your project. For this case, no additional CMake code or commands are needed to make available the CMake modules in your project.

## Usage

Having the CMake modules available in your project, it is just necessary to include the module that you want to use from your `CMakeLists.txt` file. To simplify module inclusion, it is recommended to add the modules directory to the CMake modules path variable, avoiding the need to specify the path in every inclusion, as shown below. The example considers that the project was placed in the `external` directory, and the CMake module file is named as "ModuleName.cmake":

```cmake
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/external/cmake-modules/modules")
include(ModuleName)
```

For details about how to use each CMake module, please refer to the respective CMake module file (every module provides documentation about how it can be used).

Additionally, for a concrete example of how these CMake modules can be used in an external project, see the [C++ project template](https://github.com/hugorbarbosa/cpp-project-template).

## Building

This section is only useful for the development of the CMake modules. The user of this project doesn't need to follow the build instructions of this project to utilize the modules.

This project uses CMake as its build system, with support for CMake Presets to simplify configuration and building.

For detailed build instructions, please see the [Building guide](BUILDING.md).

## License

Licensed under the [MIT license](./LICENSE).
