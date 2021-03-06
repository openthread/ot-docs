# Certification and README

Contributor: https://github.com/lmnotran

## Thread Certification

To achieve Thread Certification, the port must be tested against the official
[Thread Harness](http://graniteriverlabs.com/thread/) and pass all scenarios
listed in the Thread Certification Test Plan.

For more information, see
[Certification](https://openthread.io/certification).

## README

A detailed README is necessary to demonstrate how to build and run OpenThread on
a new hardware platform.

At a minimum, the README should include:

-   Information about the hardware platform
-   Links to the required toolchain
-   How to configure platform-specific vendor software
-   How to build and flash binaries onto the platform
-   Versions of libraries and toolchains used for validation of the port

See the [EFR32 README](https://github.com/openthread/ot-efr32/blob/main/src/README.md) for an example.
