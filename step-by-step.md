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

## 6. Project-level Pigweed Configuration

- To avoid modifying the `third_party/pigweed` submodule, we created a project-level Pigweed configuration.
- Created a `pigweed.json` file in the project root.
- Copied the contents of `third_party/pigweed/pigweed.json` into the project's `pigweed.json`.
- In the project's `pigweed.json`:
  - Added the `pw_packages` key to the `pw_env_setup` section with `mcuxpresso` as a package.
  - Changed `relative_pigweed_root` to `"third_party/pigweed"`.
  - Changed `gn_root` to `"third_party/pigweed"`.
  - Updated the paths for `project_actions`, `cipd_package_files`, `requirements`, and `constraints` to be relative to the project root (i.e., prepended `third_party/pigweed/`).
- Created a `bootstrap.sh` in the project root that sets `PW_PROJECT_ROOT` and then sources the pigweed bootstrap script.
- Created a `build_overrides` directory in the project root.
- Ran the project's `bootstrap.sh` to install the `mcuxpresso` package and its dependencies, including the `arm-none-eabi-gcc` toolchain.
- Verified that the toolchain was installed by running `source activate.sh && arm-none-eabi-gcc --version`.

**Note on MCUXpresso SDK:** The NXP MCUXpresso SDK is required for the MIMXRT595-EVK target. The `pw package` system, configured in our project-level `pigweed.json`, automatically downloads and installs the SDK into the `environment/packages/mcuxpresso` directory. This is the recommended approach over manually downloading and placing it in `third_party/mcuxpresso/sdk`.

