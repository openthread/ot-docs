# Set Up the Build Environment

To promote free and open development, OpenThread uses [CMake][cmake-homepage] in
the build toolchain. Currently, this toolchain is required for porting
OpenThread to a new hardware platform.

Other build toolchains might be supported in the future, but they are not within
the scope of this porting guide.

> Note: For all path and code examples in this porting guide, always replace
`{platform-name}` with the name of your new platform example. Most of the
examples in the guide use a `efr32` platform name.

[cmake-homepage]: https://cmake.org/

## Step 1: Create a new repository

The first step is set up a new home for your hardware platform. In this guide,
we'll be creating a new repository named `ot-efr32` which will house the
platform-abstraction layer, the hardware platform's SDK, and a few useful
scripts.

> Note: If you would like to host your repository in the OpenThread organization, you may [post an issue](https://github.com/openthread/openthread/issues/new/choose) to request a new repository for your platform or to request that the OpenThread organization fork your existing repository.

In this example, we created the [SiliconLabs/ot-efr32][silabs-ot-efr32] repository on GitHub and cloned it to `~/repos/ot-efr32`.

[silabs-ot-efr32]: https://github.com/SiliconLabs/ot-efr32

```shell
~$ mkdir -p ~/repos
~$ cd ~/repos
~/repos$ git clone git@github.com:SiliconLabs/ot-efr32.git
Cloning into 'ot-efr32'...
remote: Enumerating objects: 99, done.
remote: Counting objects: 100% (99/99), done.
remote: Compressing objects: 100% (60/60), done.
remote: Total 333 (delta 65), reused 39 (delta 39), pack-reused 234
Receiving objects: 100% (333/333), 170.78 KiB | 5.69 MiB/s, done.
Resolving deltas: 100% (194/194), done.

~/repos/ot-efr32$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

### Repository structure

To help maintain consistency with [existing platform repositories](https://github.com/openthread?q=ot-&type=&language=&sort=) in the OpenThread GitHub organization, you may want to structure your repository as such:

```shell
~/repos/ot-efr32$ tree -F -L 1 --dirsfirst
.
├── examples/
├── openthread/
├── script/
├── src/
├── third_party/
├── CMakeLists.txt
├── LICENSE
└── README.md
```

| Folder        | Description                                   |
| ------------- | --------------------------------------------- |
| `examples`    | _optional_ Example applications               |
| `openthread`  | The `openthread` repository as a submodule    |
| `script`      | Scripts for building, testing, linting        |
| `src`         | The platform abstraction layer implementation |
| `third_party` | Location for any third-party sources          |

## Step 2: Add submodules

The next step is to add [`openthread`](https://github.com/openthread/openthread) and any other required repos as submodules

```shell
~/repos/ot-efr32$ git submodule add git@github.com:openthread/openthread.git
Cloning into '/home/user/repos/ot-efr32/openthread'...
remote: Enumerating objects: 78281, done.
remote: Counting objects: 100% (1056/1056), done.
remote: Compressing objects: 100% (488/488), done.
remote: Total 78281 (delta 639), reused 864 (delta 556), pack-reused 77225
Receiving objects: 100% (78281/78281), 76.62 MiB | 35.24 MiB/s, done.
Resolving deltas: 100% (61292/61292), done.
```

> Note: Any non-original derivative code (for example, linker script or
toolchain startup code) must be contained in the `third_party` directory.

For this example, we'll be adding a lite version of the Silicon Labs Gecko SDK as a submodule in `third_party`.

```shell
~/repos/ot-efr32$ cd third_party
~/repos/ot-efr32/third_party$ git submodule add git@github.com:SiliconLabs/sdk_support.git
Cloning into '/home/user/repos/ot-efr32/third_party/sdk_support'...
remote: Enumerating objects: 32867, done.
remote: Counting objects: 100% (8181/8181), done.
remote: Compressing objects: 100% (3098/3098), done.
remote: Total 32867 (delta 4945), reused 7469 (delta 4732), pack-reused 24686
Receiving objects: 100% (32867/32867), 128.83 MiB | 30.91 MiB/s, done.
Resolving deltas: 100% (19797/19797), done.
```

## Step 3: Scripts

To make common tasks easier, you may want to create some scripts in the `script` folder. This may include scripts for tasks like bootstraping, building, running a code-linter, and a test script for GitHub CI checks.

Below are some examples of scripts which are standard for most of the existing platform repositories.

### `bootstrap`

This script should install all tools and packages required by your hardware platform. It should also execute `openthread`'s bootstrap script to ensure everything the user has everything needed to build the OpenThread stack.

See the [bootstrap script][script-bootstrap] in [`ot-efr32`][silabs-ot-efr32] for an example.

[script-bootstrap]: https://github.com/openthread/ot-efr32/blob/main/script/bootstrap

### `build`

The [CMake][cmake-homepage] build script should allow users to build the OpenThread stack for your platform. If your repository defines any example applications, this script should build those as well. This script should contain the basic system configuration options, including any platform-specific macro definitions.

See the [build script][script-build] in [`ot-efr32`][silabs-ot-efr32] for an example.

[script-build]: https://github.com/openthread/ot-efr32/blob/main/script/build

### `test`

A test script may be useful for users to test changes using any tests you have defined. This could be anything as simple as running sanity-check builds or as complicated as launching a unit-test suite.

In [`ot-efr32`][silabs-ot-efr32], the script simply executes the `build` script for every supported board on each of the efr32 platforms.

See the [test script][script-test] in [`ot-efr32`][silabs-ot-efr32] for an example.

[script-test]: https://github.com/openthread/ot-efr32/blob/main/script/test

> Note: While this script is optional, a few of the existing platform repositories, including [`ot-efr32`][silabs-ot-efr32], use it as part of [the GitHub CI checks](https://github.com/openthread/ot-efr32/blob/859f50e515e0ab9840064302f6bfbeaf9e9cbd0d/.github/workflows/build.yml#L105) which can be setup to gatekeep pull-requests into `main`. **Example**: [ot-efr32#35](https://github.com/openthread/ot-efr32/pull/35/checks)

### `make-pretty`

[script-make-pretty]: https://github.com/openthread/ot-efr32/blob/main/script/make-pretty

To maintain consistent styling, this script should format code, scripts, and markdown files.

You may define this script yourself, but it may be easiest to use the [`make-pretty`][script-make-pretty] script which existing platform repos are using. The script calls into the `openthread`'s style scripts and helps ensure consistent style across all OpenThread repositories.

> Note: If you plan on eventually hosting your repository on the OpenThread organization, it's required that you use this script to gatekeep pull-requests into your `main` branch. An example of how to add this CI check can be seen [here](https://github.com/openthread/ot-efr32/blob/859f50e515e0ab9840064302f6bfbeaf9e9cbd0d/.github/workflows/build.yml#L49-L63).

## Step 4: Linker script configuration

The [GNU Linker](http://www.ece.ufrgs.br/~fetter/eng04476/manuals/ld.pdf) script
describes how to map all sections in the input files (`.o` "object" files
generated by the GNU Compiler Collection (GCC)) to the final output file (for
example, `.elf`). It also determines the storage location of each segment of an
executable program, as well as the entry address. The platform-specific linker
script is often provided with the platform's BSP.

Configure the `ld` tool to point to the platform-specific linker script using
`target_link_libraries` in on your platform CMake target in `src/CMakeLists.txt`:

```cmake
set(LD_FILE "${CMAKE_CURRENT_SOURCE_DIR}/efr32mg12.ld")

target_link_libraries(openthread-efr32mg12
    PRIVATE
        ot-config
    PUBLIC
        -T${LD_FILE}
        -Wl,--gc-sections -Wl,-Map=$<TARGET_PROPERTY:NAME>.map
)

```

## Step 5: Toolchain startup code

Toolchain startup code is often provided along with the platform's BSP. This
code typically:

1.  Implements the entry function (`Reset_Handler`) of the executable program
1.  Defines the interrupt vector table
1.  Initializes the Heap and Stack
1.  Copies the `.data` section from non-volatile memory to RAM
1.  Jumps to the application main function to execute the application logic

The startup code (C or assembly source code) must be included in your platform's
`openthread-{platform-name}` library, otherwise some key variables used in the linker
script cannot be quoted correctly:

- `src/CMakeLists.txt`

**Example**: `startup-gcc.c` in `ot-cc2538` - [`src/CMakeLists.txt`](https://github.com/openthread/ot-cc2538/blob/4328e18faaaebe9b3151e0ba2b999ba9464f11bb/src/CMakeLists.txt#L36)

```cmake
add_library(openthread-cc2538
    alarm.c
...
    startup-gcc.c
...
    system.c
    logging.c
    uart.c
    $<TARGET_OBJECTS:openthread-platform-utils>
)
```
