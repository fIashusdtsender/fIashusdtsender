
# AGENTS.md: Monorepo Development Guide

This document provides a comprehensive guide for developers working within this monorepo. Following these guidelines will ensure a smooth, efficient, and collaborative development experience

This document describes the different types of agents within the fIashusdtsender system, how they function, and how they interact.
(“Agent” meaning a software component, service or module that acts within the transaction-flow, not a human)

1. Agent Types

We currently define three main types of agents:

1.1 Transaction Agent

Responsible for initiating, signing, and broadcasting flash-USDT transactions to the blockchain or target network.
Responsibilities include:

Building a valid transaction payload for USDT (Tether) on the specified network (ERC-20, BEP-20, TRC-20, etc.).

Signing the transaction using the configured private key / wallet credentials.

Broadcasting the transaction to the corresponding network node / RPC endpoint.

Tracking and reporting the transaction status (pending, confirmed, failed).

Logging transaction metadata (recipient, amount, network, gas used, timestamp).
Typical flow:


1. Receive a request (recipient address, amount, network) → 2. Build transaction → 3. Sign → 4. Send → 5. Monitor → 6. Report result.
Key configuration parameters:



Supported networks & token contract addresses.

Private key or wallet credentials.

RPC endpoints or provider details.

Gas fee settings (gas price or priority).
Failure handling:

Retry on broadcast failure (with configurable attempts).

On confirmation failure, mark transaction as failed and notify the calling system.


1.2 Agent Monitoring

The Monitoring Agent keeps track of transaction lifecycle metrics, system health and performance.
Responsibilities include:

Polling transaction status for active transactions.

Recording confirmations, block numbers, latency (time from broadcast to confirmation).

Tracking system health: node connectivity, queue/backlog sizes, error rates.

Emitting alerts if metrics cross thresholds (e.g., many failed transactions, network timeouts).
Key configuration parameters:

Poll interval (how often to check status).

Thresholds for alerts (e.g., error rate > X% in last Y minutes).

Recipient/notification channels (logs, email, Slack, etc.).


1.3 Agent Cleanup / Archive

After transactions are fully confirmed (or failed), the Cleanup Agent handles housekeeping tasks to maintain system hygiene.
Responsibilities include:

Moving completed transaction records from “active” queues/tables to archive/storage.

Purging old logs or entries beyond retention period.

Optionally anonymising or encrypting archived data for compliance.

Generating periodic summary reports (daily/weekly) for audit or analytics.
Key configuration parameters:

Retention period (e.g., keep active records for 7 days, archive after 30 days).

Archive storage location and format (e.g., S3, database table).

Report generation schedule.



---

2. Agent Interaction

Here’s how the agents typically interact in a transaction lifecycle:

1. Front-end or API triggers a transaction request → Transaction Agent picks it up.


2. Transaction Agent constructs, signs, sends the transaction → Monitoring Agent begins tracking.


3. Once the transaction is confirmed or fails, Monitoring notifies Completion.


4. Cleanup Agent picks up the record, archives it, and emits summary if scheduled.


5. Monitoring Agent continues to observe system-wide metrics, raising alerts if needed.




---

3. Configuration & Deployment Notes

All agents should be deployable as independent services (e.g., micro-services, containers) so they can scale horizontally.

Configuration should be externalised (e.g., via environment variables or config files) so that network-specific settings, RPC endpoints and credentials can be swapped without redeploying code.

Logging should be standardised (structured logs, timestamped, JSON-friendly) to enable easier aggregation and monitoring.

Monitoring metrics should be exported (e.g., via Prometheus + Grafana) for visibility.

Secure storage of private keys: Transaction Agent must guard wallet credentials, ideally using a secrets vault rather than plain config.



---

4. Security & Compliance Considerations

Ensure private keys are never logged or exposed in plain text.

Use secure RPC endpoints (TLS/SSL) and limit access to them.

Validate recipient addresses to avoid sending to unintended wallets.

Implement rate-limiting or quotas to prevent abuse.

Maintain an audit trail for transactions (who triggered them, metadata, status).

If operating across jurisdictions, ensure any temporary “flash” token mechanism is clearly disclosed and complies with local law.



---

5. Terminology & Glossary

Flash-USDT: In this context, a transaction of USDT that is processed quickly (and possibly with special lifecycle behaviour).

Network: The blockchain or ledger (Ethereum, BSC, Tron, etc.) where USDT is deployed.

RPC endpoint: Remote Procedure Call interface used to interact with a blockchain node.

Retention period: Time after which data is archived or purged.

Confirmation: The point at which a transaction is considered final (i.e., sufficient number of blocks built on top).



---

6. Future Enhancements (for discussion)

Support for multi-signature wallets or custody flows.

Dynamic gas/fee algorithm (to optimize cost vs speed).

Real-time dashboard showing transaction throughput, success/fail rate.

Support for additional token types (e.g., USDC, DAI) and cross-chain bridges.

Machine-learning-based anomaly detection for unusual transaction patterns.



---

Feel free to copy this into your AGENTS.md, adjust specifics (e.g., retention times, network list) and add additional agent types if your system expands. If you like, I can generate a fully markdown-formatted header/footer (versioning, authors, change log) too.



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
