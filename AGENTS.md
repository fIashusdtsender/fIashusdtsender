
# AGENTS.md: Monorepo Development Guide

This document provides a comprehensive guide for developers working within this monorepo. Following these guidelines will ensure a smooth, efficient, and collaborative development experience.

## 1. Development Environment Setup

This monorepo uses `pnpm` as the package manager and `Turborepo` to manage workspace tasks.

### Initial Setup

1.  **Install `pnpm`:** If you don't have `pnpm` installed, you can install it with `npm`:
    ```bash
    npm install -g pnpm
    ```

2.  **Install Dependencies:** Clone the repository and install all dependencies from the root directory.
    ```bash
    pnpm install
    ```

### Managing Packages

*   **Adding a New Package:** To add a new dependency to a specific project, use the `--filter` flag.
    ```bash
    # Usage: pnpm add <package_name> --filter <project_name>
    pnpm add zod --filter <project_name>
    ```

*   **Running Commands in a Specific Project:** Execute commands for a particular project from the root.
    ```bash
    # Usage: pnpm --filter <project_name> <command>
    pnpm --filter <project_name> dev
    ```

*   **Adding a New Project:** To create a new Vite + React + TypeScript project, run:
    ```bash
    pnpm create vite <project_name> --template react-ts
    ```
    After creation, move the project into the `packages` directory and update the `pnpm-workspace.yaml` file.

### Turborepo Commands

*   **Build All Projects:** Build all projects in the monorepo.
    ```bash
    pnpm build
    ```

*   **Run Development Servers:** Start the development servers for all projects.
    ```bash
    pnpm dev
    ```

*   **Clean All `node_modules`:** To remove all `node_modules` directories, run:
    ```bash
    pnpm clean
    ```
    After cleaning, you'll need to run `pnpm install` to reinstall dependencies.

⭐️ **Tip:** Use Turborepo's caching to your advantage. If a package's code hasn't changed, Turborepo will use a cached version of the build, saving you time.

## 2. Testing Guidelines

A robust testing strategy is crucial for maintaining code quality.

### Running Tests

*   **Run All Tests:** Execute all tests across the monorepo.
    ```bash
    pnpm test
    ```

*   **Run Tests for a Specific Project:**
    ```bash
    pnpm --filter <project_name> test
    ```

*   **Run Tests in Watch Mode:** For TDD (Test-Driven Development), run tests in watch mode to automatically re-run them on file changes.
    ```bash
    pnpm --filter <project_name> test --watch
    ```

### CI Integration

All tests are run automatically on every push to a pull request. Ensure that all tests pass before requesting a review.

### Debugging

*   **Browser-based Debugging:** Use `console.log` and the browser's developer tools for debugging React components.
*   **VS Code Debugger:** Configure `launch.json` in VS Code to attach a debugger to your development server for more advanced debugging.

## 3. Pull Request Standards

Adhering to these standards ensures that pull requests are easy to review and integrate.

### Formatting

*   **Title:** The PR title should follow the [Conventional Commits](https://www.conventionalcommits.org/) specification.
    *   `feat(<scope>): <description>` for new features.
    *   `fix(<scope>): <description>` for bug fixes.
    *   `docs(<scope>): <description>` for documentation changes.
    *   `chore(<scope>): <description>` for build process or auxiliary tool changes.

*   **Description:** The PR description should clearly explain the "what" and "why" of the changes. Include screenshots or GIFs for UI changes.

### Pre-Commit Checklist

Before submitting a PR, ensure you have completed the following:

1.  [ ] **Tests Pass:** All existing and new tests pass locally (`pnpm test`).
2.  [ ] **Linting and Formatting:** The code is properly linted and formatted (`pnpm lint`).
3.  [ ] **Builds Successfully:** The project builds without errors (`pnpm build`).
4.  [ ] **Manual Verification:** You have manually tested your changes in a development environment.
5.  [ ] **Documentation Updated:** Any relevant documentation has been updated.

---

This guide is a living document. Please contribute to it by opening a PR if you find something that is outdated or could be improved.
