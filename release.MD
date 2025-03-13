# Release Workflow

This GitHub Actions workflow automates the release process by creating a release tarball, testing it, and publishing a GitHub release.

## Workflow Triggers

The workflow runs on:

- **Push Events on Tags**: It is triggered when a new tag matching the pattern `v*` is pushed.

## Environment Variables

- **`GITHUB_TOKEN`**: Used for authentication with the GitHub repository.
- **`tag`**: Stores the name of the pushed tag (`github.ref_name`).

## Jobs and Steps

### `release` Job

Runs on `ubuntu-latest` and executes the following steps:

1. **Install Dependencies**
   - Updates the package list and installs `libcurl4-openssl-dev`.

2. **Download the Program Version Associated with the Release**
   - Clones the repository using the tag reference for this release.

3. **Build Release Tarball**
   - Fetches the latest `htslib` release.
   - Extracts `htslib` and prepares a tarball that excludes unnecessary files.

4. **Test Release Tarball**
   - Extracts and verifies the tarball contents.
   - Builds `htslib` and the main program from source.
   - Runs unit tests to confirm correctness.

5. **Create a Draft Release on GitHub**
   - Uses the `gh release create` command to generate a draft release with the tarball.

6. **Edit Release Notes**
   - Generates formatted release notes including download instructions and build steps.
   - Updates the draft release, making it the latest release once confirmed.

## Usage

### Triggering the Workflow

To trigger this GitHub Actions workflow, follow these steps in your local Git repository:

1. Ensure your working directory is clean and up to date:
   ```bash
   git status
   git pull origin main  # or replace 'main' with your working branch
   ```

2. Create a new version tag (e.g., `v1.0.0`):
   ```bash
   git tag v1.0.0
   ```

3. Push the tag to GitHub to trigger the release workflow:
   ```bash
   git push origin v1.0.0
   ```

4. Check the **Actions** tab on GitHub to monitor the workflow execution.

5. Once the draft release is created, review it in the **Releases** section and publish it if needed.

## Notes

- Ensure that the repository contains a `Makefile` supporting `make HTSSRC=../htslib` and `make test`.
- The workflow assumes compatibility with `htslib` and Ubuntu.
- Requires GitHub CLI (`gh`) for managing releases programmatically.

## License

This workflow and associated repository code are available under the respective project license.
