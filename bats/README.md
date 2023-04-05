## Overview

BATS is a testing framework for Bash shell scripts that provides supporting libraries and helpers for customizable test automation.

## Setup

It's important to have a Rancher Desktop CI or release build installed and running with no errors before executing the BATS tests.

On Windows:

Clone the Git repository of Rancher Desktop, whether directly inside a WSl distro or on the host Win32.
If the repository will be cloned on Win32, prior to cloning it, it's important to setup the Git configuration by running the following commands:

  ```
  git config --global core.eol lf
  git config --global core.autocrlf false
  ```
Note that changing `crlf` settings is not needed when you clone it inside a WSL distro.
Regardless of the repository location, the BATS tests can be executed ONLY from inside a WSL distribution. So, if the repository is cloned on Win32, the repository can be located within a WSL distro from /mnt/c, as it represents the `C:` drive on Windows.

All platforms:
From the root directory of the Git repository, run the following commands to install BATS and its helper libraries into the BATS test directory:

  ```
  git submodule update --init
  ```

## Running BATS

1. To run the BATS test, specify the path to BATS executable from bats-core and run the following commands:

    To run a specific test set from a bats file:

      Example:

      ```
      cd bats
      ./bats-core/bin/bats tests/registry/creds.bats
      ```

    To run all BATS tests:

      ```
      cd bats
      ./bats-core/bin/bats tests/*/
      ```

    To run the BATS test, specifying some of Rancher Desktop's configuration, run the following commands:

      Example:

      ```
      cd bats
      RD_CONTAINER_RUNTIME=moby RD_USE_IMAGE_ALLOW_LIST=false ./bats-core/bin/bats tests/registry/creds.bats
      ```
    On Windows:

      BATS must be executed from within a WSL distibution. (You have to cd into /mnt/c/REPOSITORY_LOCATION from your unix shell)

## Writing BATS Tests

1. Add BATS test by creating files with `.bats` extension under ./bats/tests/FOLDER_NAME
2. A Bats test file is a Bash script with special syntax for defining test cases. BATS syntax and libraries for defining test hooks, writing assertions and treating output can be accessed via BATS [documentation](https://bats-core.readthedocs.io/en/stable/): [bats-core](https://github.com/rancher-sandbox/bats-core), [bats-assert](https://github.com/rancher-sandbox/bats-assert), [bats-file](https://github.com/rancher-sandbox/bats-file), [bats-support](https://github.com/rancher-sandbox/bats-support)

## BATS linting

After finishing to develop a BATS test suite, you can locally verify the syntax and formatting feedback by linting prior to submitting a PR, following the instructions:

  1. Make sure to have installed `shellcheck` and `shfmt`

    On MacOS:

        - Assuming you have Homebrew:

          ```
          brew install shfmt shellcheck
          ```
        - If you have Go installed, you can also install `shfmt` by running:

          ```
          go install mvdan.cc/sh/v3/cmd/shfmt@v3.6.0
          ```

    On Linux:

        - The simplest way to install ShellCheck locally is through your package managers such as `apt/apt-get/yum`. Run commands as per your distro.

          ```
          sudo apt install shellcheck
          ```

        - `shfmt` is available as a snap application. If your distribution has snap installed, you can install `shfmt` using the command:

          ```
          sudo snap install shfmt
          ```
          The other way to install `shfmt` is by using the following one-liner command:

          ```
          curl -sS https://webinstall.dev/shfmt | bash
          ```
          If you have Go installed, you can also install `shfmt` by running:

          ```
          go install mvdan.cc/sh/v3/cmd/shfmt@v3.6.0
          ```
    On Windows:

        - The simplest way to install `shellcheck` locally is:

          Via chocolatey:

            ```
            choco install shellcheck
            ```
          Via scoop:

            ```
            scoop install shellcheck
            ```
        - If you have Go installed, you can install `shfmt` by running:

          ```
          go install mvdan.cc/sh/v3/cmd/shfmt@v3.6.0
          ```

  2. Get the syntax and formatting feedback for BATS linting by running from the root directory of the Git repository:

      ```
      make -C bats lint
      ```
  3. Please, make sure to fix the highlighted linting errors prior to submitting a PR. You can automatically apply formatting changes suggested by `shfmt` by running the following command:

    Example
    ```
    shfmt -w ./bats/tests/containers/factory-reset.bats
    ```