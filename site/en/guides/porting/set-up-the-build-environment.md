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
we'll be creating a new repo named `ot-efr32` which will house the
platform-abstraction layer, the hardware platform's SDK, and a few useful
scripts.

[//]: # (TODO: Do we want to demonstrate making a new git repo through GitHub?)


```shell
mason@bigboi:~$ mkdir -p ~/repos
mason@bigboi:~$ cd ~/repos
mason@bigboi:~/repos$ git clone git@github.com:SiliconLabs/ot-efr32.git
Cloning into 'ot-efr32'...
remote: Enumerating objects: 99, done.
remote: Counting objects: 100% (99/99), done.
remote: Compressing objects: 100% (60/60), done.
remote: Total 333 (delta 65), reused 39 (delta 39), pack-reused 234
Receiving objects: 100% (333/333), 170.78 KiB | 5.69 MiB/s, done.
Resolving deltas: 100% (194/194), done.

mason@bigboi:~/repos/ot-efr32$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

## Step 2: Download boiler-plate files

[//]: # (TODO: Should we create a template repo that contains everything needed for porting? I'm thinking it would include a stubbed out PAL, stubbed out READMEs, CMake files, scripts, etc. This would be a good way to keep the structure of these new repos relatively consistent with the existing repos.)

To give the new repo a jumpstart, go to [`ot-stubbed-platform`], download the repo as a `.zip` file, and extract it to your repo.

```shell
# TODO: This is just pseudocode at this point since the repo hasn't actually been created.
mason@bigboi:~/repos/ot-efr32$ wget https://github.com/SiliconLabs/ot-stubbed-platform/archive/refs/heads/main.zip
mason@bigboi:~/repos/ot-efr32$ unzip main.zip
mason@bigboi:~/repos/ot-efr32$ ls -alh
total 64K
drwxr-xr-x 10 mason www-data 4.0K Apr 13 16:17 .
drwxr-xr-x 14 mason www-data 4.0K Apr 16 16:46 ..
drwxr-xr-x  4 mason www-data 4.0K Apr 16 16:29 build
-rw-r--r--  1 mason www-data 3.2K Apr 12 18:19 .clang-format
-rw-r--r--  1 mason www-data 3.1K Apr 12 18:19 CMakeLists.txt
drwxr-xr-x  3 mason www-data 4.0K Apr 12 18:19 examples
drwxr-xr-x  9 mason www-data 4.0K Apr 13 16:49 .git
drwxr-xr-x  3 mason www-data 4.0K Apr 12 18:19 .github
-rw-r--r--  1 mason www-data 1.4K Apr 12 18:19 .gitignore
-rw-r--r--  1 mason www-data  234 Apr 12 18:19 .gitmodules
-rw-r--r--  1 mason www-data 1.5K Apr 12 18:19 LICENSE
drwxr-xr-x 12 mason www-data 4.0K Apr 13 16:49 openthread
-rw-r--r--  1 mason www-data 2.3K Apr 12 18:19 README.md
drwxr-xr-x  2 mason www-data 4.0K Apr 12 18:19 script
drwxr-xr-x  7 mason www-data 4.0K Apr 12 18:19 src
drwxr-xr-x  4 mason www-data 4.0K Apr 12 18:19 third_party
```

## Step 3: Add submodules

The next step is to add [`openthread`](https://github.com/openthread/openthread) and any other required repos as submodules

```shell
mason@bigboi:~/repos/ot-efr32$ git submodule add git@github.com:openthread/openthread.git
Cloning into '/home/mason/repos/ot-efr32/openthread'...
remote: Enumerating objects: 78281, done.
remote: Counting objects: 100% (1056/1056), done.
remote: Compressing objects: 100% (488/488), done.
remote: Total 78281 (delta 639), reused 864 (delta 556), pack-reused 77225
Receiving objects: 100% (78281/78281), 76.62 MiB | 35.24 MiB/s, done.
Resolving deltas: 100% (61292/61292), done.
```

> Note: Any non-original derivative code (for example, linker script or
toolchain startup code) must be contained in the `third_party` directory.

For this example, we'll be adding a lite version of the Silicon Labs Gecko SDK as a submodule.

```shell
mason@bigboi:~/repos/ot-efr32$ cd third_party
mason@bigboi:~/repos/ot-efr32/third_party$ git submodule add git@github.com:SiliconLabs/sdk_support.git
Cloning into '/home/mason/repos/ot-efr32/third_party/sdk_support'...
remote: Enumerating objects: 32867, done.
remote: Counting objects: 100% (8181/8181), done.
remote: Compressing objects: 100% (3098/3098), done.
remote: Total 32867 (delta 4945), reused 7469 (delta 4732), pack-reused 24686
Receiving objects: 100% (32867/32867), 128.83 MiB | 30.91 MiB/s, done.
Resolving deltas: 100% (19797/19797), done.
```

## Step 4: Modifying scripts

As part of the boiler-plate files downloaded in a previous step, the `script` folder contains useful scripts for common tasks like bootstraping, building, running a code-linter, and a test script for GitHub CI checks.



### `script/bootstrap`

This script allows users to install all tools and packages required by your hardware platform. It also executes `openthread`'s bootstrap script, ensuring everything the user has everything needed to build.

[//]: # (TODO add link to file in `ot-stubbed-platform`)


### `script/build`

The [CMake][cmake-homepage] build script contains the basic system configuration options, including any platform-specific macro definitions.

[//]: # (TODO add more)
[//]: # (TODO add link to file in `ot-stubbed-platform`)

### `script/test`

[//]: # (TODO add link to file in `ot-stubbed-platform`)

### `script/make-pretty`

[//]: # (TODO add description)



### Linker script configuration

[//]: # (TODO This should be moved somewhere else)

The [GNU Linker](http://www.ece.ufrgs.br/~fetter/eng04476/manuals/ld.pdf) script
describes how to map all sections in the input files (`.o` "object" files
generated by the GNU Compiler Collection (GCC)) to the final output file (for
example, `.elf`). It also determines the storage location of each segment of an
executable program, as well as the entry address. The platform-specific linker
script is often provided with the platform's BSP.

Configure the `ld` tool to point to the platform-specific linker script using
the `-T` option of the `LDADD_COMMON` variable.

Create
`/openthread/examples/platforms/{platform-name}/Makefile.platform.am`
and point the new platform to its linker script:

```
if OPENTHREAD_EXAMPLES_EFR32
    LDADD_COMMON                                                      += \
    $(top_builddir)/examples/platforms/efr32/libopenthread-efr32.a       \
    $(top_srcdir)/third_party/silabs/gecko_sdk_suite/v1.0/platform/radio/rail_lib/autogen/librail_release/librail_efr32xg12_gcc_release.a \
    $(NULL)

LDFLAGS_COMMON                                                        += \
    -T $(top_srcdir)/third_party/silabs/gecko_sdk_suite/v1.0/platform/Device/SiliconLabs/EFR32MG12P/Source/GCC/efr32mg12p.ld \
    $(NULL)
endif # OPENTHREAD_EXAMPLES_EFR32
```

Add the platform's linker script configuration to the
[`/openthread/examples/platforms/Makefile.platform.am`](https://github.com/openthread/openthread/blob/main/examples/platforms/Makefile.platform.am)
utility Makefile:

```
if OPENTHREAD_EXAMPLES_EFR32
include $(top_srcdir)/examples/platforms/efr32/Makefile.platform.am
endif
```


### Toolchain startup code

[//]: # (TODO Rewrite for CMake. Might not even be needed)

Toolchain startup code is often provided along with the platform's BSP. This
code typically:

1.  Implements the entry function (`Reset_Handler`) of the executable program
1.  Defines the interrupt vector table
1.  Initializes the Heap and Stack
1.  Copies the `.data` section from non-volatile memory to RAM
1.  Jumps to the application main function to execute the application logic

The startup code (C or assembly source code) must be added to the
platform-specific `Makefile.am`, otherwise some key variables used in the linker
script cannot be quoted correctly:

-   `/openthread/examples/platforms/{platform-name}/Makefile.am`

Example:

```
libopenthread_efr32_a_SOURCES   =  \
@top_builddir@/third_party/silabs/gecko_sdk_suite/v1.0/hardware/kit/common/bsp/bsp_bcc.c \
@top_builddir@/third_party/silabs/gecko_sdk_suite/v1.0/hardware/kit/common/bsp/bsp_stk.c \
@top_builddir@/third_party/silabs/gecko_sdk_suite/v1.0/platform/Device/SiliconLabs/EFR32MG12P/Source/system_efr32mg12p.c \
@top_builddir@/third_party/silabs/gecko_sdk_suite/v1.0/platform/Device/SiliconLabs/EFR32MG12P/Source/GCC/startup_efr32mg12p.c \
```

