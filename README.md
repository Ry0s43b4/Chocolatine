# Chocolatine

A robust Continuous Integration and Continuous Deployment (CI/CD) workflow automated via **GitHub Actions**, developed as part of the **Epitech** DevOps curriculum.

## Project Overview

The objective of **Chocolatine** is to set up a standardized automated workflow (`chocolatine.yml`) to enforce development quality control before code reaches production or the final delivery repository. 

It automatically monitors repository hygiene, verifies compliance with the Epitech coding style, ensures proper program compilation, executes unit tests, and mirrors validated code to an external target.

## Workflow Jobs & Features

The CI/CD pipeline consists of 4 sequential jobs triggered upon every `push` and `pull_request`:

1. **Check Repository Cleanliness (`check_repository_cleanliness`)**
   * Scans the workspace for unwanted or backup files (e.g., files starting or ending with `#`, ending with `~`, or containing `tmp` extensions).
   * Fails immediately if the repository structure is compromised.

2. **Check Coding Style (`check_coding_style`)**
   * Leverages the official Epitech Coding Style Checker Docker image (`ghcr.io/epitech/coding-style-checker`).
   * Assesses code compliance and outputs errors. Fails if major violations are encountered.

3. **Check Program Compilation (`check_program_compilation`)**
   * Triggers the project's `Makefile` to ensure successful compilation.
   * Verifies that all expected binaries listed in the `EXECUTABLES` environment variable are correctly generated and executable.

4. **Run Tests (`run_tests`)**
   * Executes unit tests via `make tests_run` to maintain high test coverage and prevent regression.

5. **Push to Mirror (`push_to_mirror`)**
   * *Trigger:* Runs exclusively on a `push` event to the `main`/`master` branch, provided all previous steps pass.
   * Automatically synchronizes and pushes the validated repository to the Epitech delivery remote url (`MIRROR_URL`).

## Configuration & Environment Variables

To operate seamlessly, the workflow relies on several environment variables defined at the repository level:

* `MIRROR_URL` – The target Git URL of the official Epitech submission repository.
* `EXECUTABLES` – A comma-separated list of compiled binaries expected to be produced (e.g., `pushswap` or `my_hunter`).

### GitHub Secrets Required

To secure sensitive deployment credentials without hardcoding them, ensure the following secret is registered in your repository's settings:
* `GIT_SSH_PRIVATE_KEY` – A valid private SSH key paired with write permissions on the mirror destination.

## Getting Started

### Project Structure

```text
.
├── .github/
│   └── workflows/
│       └── chocolatine.yml     # Main GitHub Actions configuration
├── src/
├── tests/
├── Makefile
└── README.md
```

### Setup

1. Copy the `.github/workflows/chocolatine.yml` file into your current development repository.
2. Go to your GitHub repository -> **Settings** -> **Secrets and variables** -> **Actions**.
3. Add the required **Secrets** (`GIT_SSH_PRIVATE_KEY`) and **Variables** (`MIRROR_URL`, `EXECUTABLES`).
4. Push a change to trigger the automated verification pipeline.

---
Automate with confidence! 🚀
