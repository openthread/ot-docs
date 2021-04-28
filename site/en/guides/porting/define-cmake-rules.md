# Define CMake rules

The CMake build system relies on `CMakeLists.txt` files which define build targets. The `CMakeLists.txt` at the root of your repository is the top of the build tree and is a good place to start defining the various targets, options, and macros which will be used in the build process.

The very first values to define are the project name and the supported platforms.

In this example, we set the project name to `ot-efr32` with a version of `0.0.1`. We also define a variable `EFR32_PLATFORM_VALUES` which is a list of `efr32` platforms supported by `ot-efr32`. For the sake of this example, we've defined multiple platforms, but having a single platform for the `_PLATFORM_VALUES` variable is fine as well.

```cmake
cmake_minimum_required(VERSION 3.10.2)
project(ot-efr32 VERSION 0.0.1)

set(EFR32_PLATFORM_VALUES
    "efr32mg1"
    "efr32mg12"
    "efr32mg13"
    "efr32mg21"
)
```

The `CMakeLists.txt` file includes a check that will prevent a build launched for an unsupported platform.
```cmake
set_property(CACHE EFR32_PLATFORM PROPERTY STRINGS ${EFR32_PLATFORM_VALUES})
if(NOT EFR32_PLATFORM IN_LIST EFR32_PLATFORM_VALUES)
    message(FATAL_ERROR "Please select a supported platform: ${EFR32_PLATFORM_VALUES}")
endif()
```

The next variable which needs to be defined is `OT_PLATFORM_LIB`. This variable is used by the OpenThread example applications to link against your platform.

```cmake
set(OT_PLATFORM_LIB "openthread-${EFR32_PLATFORM}")
```


## OpenThread CMake options

> Note: This section is optional

Various features in OpenThread may be enabled/disabled/configured by defining CMake variables.

On the `ot-efr32` platform, an external mbedTLS library `silabs-mbedtls` is used.

```cmake
set(OT_BUILTIN_MBEDTLS_MANAGEMENT OFF CACHE BOOL "disable builtin mbedtls management" FORCE)
set(OT_EXTERNAL_MBEDTLS "silabs-mbedtls" CACHE STRING "use silabs mbedtls" FORCE)
set(OT_MBEDTLS ${OT_EXTERNAL_MBEDTLS})
```
> Note: For a list of OpenThread CMake options available, refer to [`openthread/etc/cmake/options.cmake`](https://github.com/openthread/openthread/blob/main/etc/cmake/options.cmake).


## Define output directories

> Note: This section is optional

The output directory for different target files can be configured using the variables below.

```cmake
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)
```

> Note: Refer to [CMake's documentation](https://cmake.org/cmake/help/latest/manual/cmake-variables.7.html) for a list of all variables provided or used by CMake.


## Add `openthread` to the build tree

To include the `openthread` submodule in the build tree

```cmake
add_subdirectory(openthread)
```

### Passing build properties to OpenThread

The final section of this file allows you to define build properties (definitions, options, include dirs, etc) which you may want to add to the `openthread` build tree and to your platform libraries.

A convenient way to add these definitions is by using the `ot-config` target. This target is a phony target which is used solely for the purpose of defining configuration and is linked against by almost all CMake targets in `openthread`.

```cmake
# Define config filename macros
target_compile_definitions(ot-config INTERFACE
    OPENTHREAD_CONFIG_FILE="openthread-core-efr32-config.h"
    OPENTHREAD_PROJECT_CORE_CONFIG_FILE="openthread-core-efr32-config.h"
    OPENTHREAD_CORE_CONFIG_PLATFORM_CHECK_FILE="openthread-core-efr32-config-check.h"
)

# Disable -Wshadow and -Wpedantic
target_compile_options(ot-config INTERFACE
    -Wno-shadow
    -Wno-pedantic
)

# Add platform dirs to "include" dirs
target_include_directories(ot-config INTERFACE
    ${PROJECT_SOURCE_DIR}/src/src
    ${PROJECT_SOURCE_DIR}/src/${EFR32_PLATFORM}
    ${PROJECT_SOURCE_DIR}/src/${EFR32_PLATFORM}/crypto
)
```

## Adding subdirectories to the build tree

Now that the top-level configuration is defined, it's time to add other subdirectories to the build tree.

The `src` folder is where the platform-abstration layer source code will be located. The `third_party` folder will contain any third-party code.

If you would like to include sample applications in the repo, create an `examples` folder and add it to the build tree as well.

```cmake
add_subdirectory(src)
add_subdirectory(third_party)

# Optional
add_subdirectory(examples)
```

## The `src` directory

This is the heart of the repository and is where the platform abstraction layer will be implemented. It will also contain a few other required files.

### `src/arm-none-eabi.cmake`

This is a toolchain file which defines some variables that CMake will use in the build process. A good starting point would be to copy [the file](https://github.com/openthread/ot-efr32/blob/main/src/arm-none-eabi.cmake) from `ot-efr32` and modify it to fit your platform.


### `src/CMakeLists.txt`

This is where you will define the source files, include paths, compiler flags, etc. for your platform library. A good example of this file for a repository which supports only a single platform is from [`ot-cc2538`](https://github.com/openthread/ot-cc2538/blob/main/src/CMakeLists.txt).

Support for multiple platforms in a single repository is possible and several platform repositories can be used as reference. See [ot-efr32][ot-efr32] and [ot-nrf528xx][ot-nrf528xx].

[ot-efr32]: https://github.com/openthread/ot-efr32
[ot-nrf528xx]: https://github.com/openthread/ot-nrf528xx
