# Step-by-step Guide

This document records the major steps taken to set up the Bazel project with Pigweed.

## 1. Initialize Git Repository

- Initialized a new Git repository using `git init`.

## 2. Add Pigweed Submodule

- Added the Pigweed repository as a Git submodule to `third_party/pigweed`.
- Initialized the submodule.

## 3. Set up Bazel Workspace

- Created a `.bazelrc` file to enable C++20 support.
- Created a `MODULE.bazel` file to define the project and its dependencies, using Bzlmod.
- Pointed to the local Pigweed submodule using `local_path_override`.
- Registered the Pigweed toolchains.
- Created an empty `WORKSPACE` file.

## 4. Create `apps/hello_world`

- Created a `hello_world.cc` file with a simple "Hello, Pigweed!" message.
- Created a `BUILD.bazel` file to build the `hello_world` application as a `cc_binary`.
- Added a dependency on `@pigweed//pw_log`.

## 5. Build the Application

- Built the `hello_world` application using `bazel build //apps/hello_world`.
- Added `rules_cc` as a dependency in `MODULE.bazel` to fix a build error.
- The build was successful.
