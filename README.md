# Simple Python Workflow with Reusable GitHub Actions

This project is a minimal Python application designed to demonstrate how to use reusable workflows in GitHub Actions, including passing custom parameters and automating pull request creation.

## Project Structure

```
reusable-workflows/
├── src/
│   └── main.py
├── .github/
│   └── workflows/
│       ├── reusable-workflow.yml
│       └── example-using-reusable.yml
├── requirements.txt
└── README.md
```

## Getting Started

1. **Clone the repository**:
   ```zsh
   git clone <repository-url>
   cd reusable-workflows
   ```

2. **Install dependencies**:
   ```zsh
   pip install -r requirements.txt
   ```

3. **Run the application locally**:
   ```zsh
   python src/main.py
   ```

## Code Overview

- `src/main.py`: Simple script that prints "Hello, World!".
- `requirements.txt`: Lists dependencies (`Flask`, `pytest`).
- `.github/workflows/reusable-workflow.yml`: Defines a reusable workflow for Python setup, running the app, and optionally creating a PR.
- `.github/workflows/example-using-reusable.yml`: Example workflow that calls the reusable workflow with parameters.

## Reusable Workflow Details

The reusable workflow (`.github/workflows/reusable-workflow.yml`) can:
- Set up Python
- Install dependencies
- Run the application
- Optionally create a pull request (PR) when triggered with parameters

### Inputs
- `create-pr` (boolean): If true, creates a PR.
- `pr-base` (string): Target branch for the PR (default: `main`).
- `pr-head` (string): Source branch for the PR (default: `auto-pr-branch`).
- `pr-title` (string): Title for the PR.

## Example: Using the Reusable Workflow

The example workflow (`.github/workflows/example-using-reusable.yml`) demonstrates how to call the reusable workflow with custom parameters:

```yaml
name: Example Using Reusable Workflow

on:
  push:
    branches-ignore: [main]

jobs:
  call-reusable:
    uses: ./.github/workflows/reusable-workflow.yml
    with:
      create-pr: true
      pr-base: 'main'
      pr-head: 'auto-pr-${{ github.ref_name }}'
      pr-title: 'Automated PR for branch ${{ github.ref_name }}'
```

- When you push to any branch except `main`, this workflow will run and create a PR back to the repository using the specified branch and title.

## How It Works
1. Push to a feature branch (not `main`).
2. The example workflow triggers and calls the reusable workflow.
3. The reusable workflow runs the Python app and, if `create-pr` is true, creates a PR using the [peter-evans/create-pull-request](https://github.com/peter-evans/create-pull-request) action.

## Customization
- You can adjust the parameters in the example workflow to fit your needs.
- The reusable workflow can be called from other repositories or workflows as well.

---

This project is a foundation for learning about reusable workflows and automation in CI/CD with GitHub Actions.