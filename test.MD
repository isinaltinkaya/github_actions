# Test Workflow

A GitHub Actions workflow to automatically build and test the project upon specific events.

## Workflow Triggers

The workflow runs on the following events:

- **Push Events**: When changes are pushed to the `main` or `develop` branches.
- **Pull Request Events**: When a pull request is opened or updated targeting the `main` branch.
- **Manual Dispatch**: It can be manually triggered using `workflow_dispatch` with an optional `git-ref` input specifying a Git SHA-1 commit hash.

## Jobs and Steps

### `build` Job
The `build` job runs on `ubuntu-latest` and executes the following steps:

1. **Checkout Repository**
   - Uses the `actions/checkout@v3` action to clone the repository.

2. **Install Dependencies**
   - Updates the package list and installs required dependencies:
     - `libcurl4-openssl-dev`
     - `libbz2-dev`

3. **Install HTSlib**
   - Installs `libhts3` and `libhts-dev` packages, required for handling genomic data.

4. **Compile Program**
   - Runs `make HTSSRC="systemwide"` to compile the program using the system-wide HTSlib installation.

5. **Run Unit Tests**
   - Executes `make test` to validate the implementation.

## Usage

To manually trigger the workflow, go to the repository's **Actions** tab, select the workflow, and use the `Run workflow` button. Optionally, provide a Git SHA-1 commit hash as input.

## Notes
- Ensure that your repository includes a `Makefile` with `make test` defined.
- The workflow assumes that the required dependencies are available on Ubuntu.

## License
This repository and workflow are open-source and available under the respective project license.
