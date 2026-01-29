# PR Security Pipeline

This repository uses a Pull Request (PR) based security and quality pipeline implemented using GitHub Actions. The pipeline is designed to validate code changes before they are merged into shared branches.

## Purpose of the PR Pipeline

The PR pipeline acts as a quality gate. Its responsibility is to identify security risks and dependency issues early in the development lifecycle and provide actionable feedback to developers before code is merged.

## What Triggers the Pipeline

The pipeline runs only on Pull Request events. It does not run on direct pushes. This ensures that validation happens only when code is proposed for merging.

## Key Controls Implemented in the PR Pipeline

### 1. Minimal Permissions

The pipeline uses least-privilege permissions. It only has read access to repository contents and permission to publish security findings. This reduces security risk, especially for forked pull requests.

### 2. Concurrency Control

If multiple commits are pushed to the same pull request, older pipeline runs are automatically cancelled. This prevents outdated results and reduces CI resource usage.

### 3. Timeout Management

Timeouts are applied at both job and step levels. This ensures the pipeline does not hang indefinitely due to network issues, tool failures, or slow external services.

### 4. Secret Scanning

Gitleaks is used to scan the pull request for exposed secrets. If any credentials or sensitive data are detected, the pipeline fails immediately. Secret scan results are available in the workflow logs.

### 5. Static Code Security Scanning with SARIF

Semgrep is used for static code analysis. The scan results are generated in SARIF format and uploaded to GitHub Code Scanning. This allows developers to review findings directly in the pull request with file-level annotations.

### 6. Dependency Vulnerability Scanning

OWASP Dependency Check scans third-party dependencies for known vulnerabilities. If vulnerabilities with a CVSS score of 7 or higher are found, the pipeline fails. A detailed HTML report is generated.

### 7. Reporting and Artifacts

The dependency vulnerability report is uploaded as a workflow artifact. This allows the report to be reviewed and downloaded even if the pipeline fails. SARIF results are visible through GitHub’s Security and Code Scanning interface.

## How to Review Pipeline Results

- Secret scan results can be found in the GitHub Actions workflow logs.
- Static code security findings are available in the repository under Security → Code scanning and as annotations in the pull request.
- Dependency vulnerability reports can be downloaded from the Actions workflow artifacts section.

## Why This Pipeline Design Is Used

This PR pipeline enforces security and quality early, follows GitHub Actions best practices, uses only free and open-source tools, and aligns with enterprise DevSecOps standards. It ensures fast feedback, strong governance, and clear visibility for developers and reviewers.
