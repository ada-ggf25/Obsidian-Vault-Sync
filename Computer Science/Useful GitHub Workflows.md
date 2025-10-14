#computer_science #github #git 

Here’s a tight, high-signal list of Actions that most Python/ML repos find useful, grouped by use-case. I’ve included the canonical action name and a one-liner on when to use it (plus a tiny snippet where it helps).

# CI basics

- **actions/checkout** – pull your repo to the runner (you’re already using this).
    
- **actions/setup-python** – choose Python version; add `cache: pip` to speed installs.
    
- **actions/cache** – cache anything custom (e.g., `.venv`, `.mypy_cache`, `~/.cache/pip`).
    
    ```yaml
    - uses: actions/cache@v4
      with: { path: ~/.cache/pip, key: pip-${{ runner.os }}-${{ hashFiles('**/requirements*.txt') }} }
    
    ```
    
- **actions/upload-artifact** / **download-artifact** – persist test reports, wheels, coverage, model artifacts between jobs.
    

# Python quality toolchain

- **psf/black** (via `pip` + run) or **psf/black@stable** – auto-format.
    
- **astral-sh/ruff-action** – ultra-fast linting.
    
- **mypy** (via `pip` + run) – static typing checks.
    
- **pre-commit/action** – run your `.pre-commit-config.yaml` hooks in CI so local ≈ CI.
    
    ```yaml
    - uses: pre-commit/action@v3.0.1
    
    ```
    
- **pytest** (via `pip` + run) + **codecov/codecov-action** – test + upload coverage.
    
    ```yaml
    - run: pytest --cov=your_pkg --cov-report=xml
    - uses: codecov/codecov-action@v4
    
    ```
    

# Security & compliance

- **github/codeql-action** – code scanning (SAST) for Python/JS/etc.
    
    ```yaml
    - uses: github/codeql-action/init@v3
    - uses: github/codeql-action/analyze@v3
    
    ```
    
- **actions/dependency-review-action** – fail PRs that introduce known-vuln deps.
    
- **trufflesecurity/trufflehog** – secret scanning beyond GitHub’s native checks.
    

# Releases & packaging

- **pypa/gh-action-pypi-publish** – publish to PyPI (with a prior build step).
    
    ```yaml
    - run: python -m build
    - uses: pypa/gh-action-pypi-publish@release/v1
      with: { password: ${{ secrets.PYPI_API_TOKEN }} }
    
    ```
    
- **softprops/action-gh-release** – attach binaries/wheels to GitHub Releases.
    
- **actions/create-release** – create a tagged release from CI.
    
- **python-semantic-release/python-semantic-release** – automated versioning + changelogs from Conventional Commits.
    

# Docs & Pages

- **actions/setup-python** + `pip install mkdocs-material`/**Sphinx** – build docs.
    
- **actions/upload-pages-artifact** + **actions/deploy-pages** – publish to GitHub Pages.
    
    ```yaml
    - run: mkdocs build
    - uses: actions/upload-pages-artifact@v3
    - uses: actions/deploy-pages@v4
    
    ```
    

# Containers & deployment

- **docker/login-action** + **docker/build-push-action** – build/push Docker images.
    
    ```yaml
    - uses: docker/build-push-action@v6
      with: { push: true, tags: ghcr.io/owner/app:${{ github.sha }} }
    
    ```
    
- **azure/**, **google-github-actions/**, **aws-actions/** – cloud logins & deploys (AKS/GKE/EKS, Functions/Cloud Run/Lambda).
    

# PR automation & repo hygiene

- **actions/labeler** – auto-label PRs by path rules.
- **peter-evans/create-pull-request** – open PRs from CI (e.g., update lockfiles).
- **actions/stale** – auto-mark/close inactive issues/PRs.
- **danger/danger-js** – custom PR checks/comments (lint summaries, changelog notes).
- **actions/first-interaction** – greet first-time contributors.

# Monorepo / multi-env extras

- **gradle/gradle-build-action**, **actions/setup-node**, **actions/setup-java** – if you have polyglot builds.
    
- **dorny/paths-filter** – run jobs only when certain folders change (great for monorepos).
    
    ```yaml
    - uses: dorny/paths-filter@v3
      id: filter
      with:
        filters: |
          py: { paths: ['src/**', 'tests/**', 'pyproject.toml'] }
    
    ```
    

# Nice workflow features to adopt

- **Matrix builds**: test on `3.10–3.13` and OSes as needed.
    
- **Reusable workflows**: DRY up common CI across repos (`workflow_call`).
    
- **permissions:** set least privilege:
    
    ```yaml
    permissions: { contents: read, id-token: write }
    
    ```
    
- **concurrency:** cancel superseded runs on same branch:
    
    ```yaml
    concurrency: { group: ${{ github.ref }}, cancel-in-progress: true }
    
    ```
    

If you tell me your stack (docs tool, packaging method, deploy target), I’ll sketch a minimal end-to-end CI that lint/tests, builds docs, publishes a wheel, and pushes a Docker image—all with caching and required checks.

---

Great topics—these are the “power features” of Actions. Here’s a fast, example-first tour.

# Triggers

## 1) Cron jobs (`schedule`)

- Runs on a UTC cron, even if your repo shows a different timezone.

```yaml
on:
  schedule:
    # Every day at 07:30 UTC
    - cron: "30 7 * * *"

```

Tip: if you need local time, convert to UTC (e.g., London = UTC or UTC+1 with DST).

## 2) Manual runs (`workflow_dispatch`)

- Lets you start a workflow from the UI or API. You can define inputs.

```yaml
on:
  workflow_dispatch:
    inputs:
      env:
        description: "Target environment"
        type: choice
        options: [dev, staging, prod]
        default: dev
      force:
        description: "Force deploy?"
        type: boolean
        default: false
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Env=${{ inputs.env }}, Force=${{ inputs.force }}"

```

# Job strategy

## 3) Matrix + `fail-fast`

- Matrix runs N variants in parallel. `fail-fast: false` keeps others running if one fails.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        python: ["3.10", "3.11", "3.12", "3.13"]
        os: [ubuntu-latest, macos-latest]
        include:
          - os: windows-latest
            python: "3.12"   # add a specific combo
        exclude:
          - os: macos-latest
            python: "3.10"   # skip a combo
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: ${{ matrix.python }} }
      - run: python -V && echo "${{ matrix.os }}"

```

# Conditional logic

## 4) Step/job `if:` conditions

- Gate a step or entire job on expressions.

```yaml
jobs:
  lint:
    if: ${{ github.event_name == 'pull_request' }}   # skip on push
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Only on PRs"

  build:
    runs-on: ubuntu-latest
    steps:
      - run: make build
      - name: Upload logs even if build fails
        if: ${{ always() }}
        uses: actions/upload-artifact@v4
        with: { name: logs, path: logs/ }

```

Common functions: `success()`, `failure()`, `cancelled()`, `always()`.

Common helpers: `contains()`, `startsWith()`, `endsWith()`.

# Passing data between steps/jobs

## 5) Step outputs

- Use a step `id` and write `name=value` lines to `$GITHUB_OUTPUT`.

```yaml
steps:
  - id: compute
    run: |
      echo "sha_short=${GITHUB_SHA::7}" >> "$GITHUB_OUTPUT"
  - run: echo "Short SHA is ${{ steps.compute.outputs.sha_short }}"

```

## 6) Job outputs

- Expose a value from one job to others (needs `needs:`).

```yaml
jobs:
  prep:
    runs-on: ubuntu-latest
    outputs:
      tag: ${{ steps.meta.outputs.tag }}
    steps:
      - id: meta
        run: echo "tag=v1.${GITHUB_RUN_NUMBER}" >> "$GITHUB_OUTPUT"

  deploy:
    needs: prep
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying ${{ needs.prep.outputs.tag }}"

```

# Self-hosted runners

## 7) Using `self-hosted` runners

```yaml
jobs:
  heavy:
    runs-on: self-hosted  # or: [self-hosted, linux, gpu]

```

**Key points:**

- **Registration & labels:** Install the GitHub runner binary on your machine/VM, register it to your org/repo, add labels (e.g., `gpu`, `arm64`) and target them in `runs-on`.
- **Security:** Workflows from forks can run untrusted code. Avoid exposing secrets/filesystems on broad `self-hosted` pools. Use **private repositories**, **restricted repositories**, or **“Require approval for first-time contributors”** on PRs.
- **Isolation:** Prefer ephemeral runners (one-job VMs/containers that self-destruct) to avoid cross-job contamination. Popular setups: actions-runner-controller (Kubernetes), GitHub Runner Scale Sets (Azure), or custom autoscaling images.
- **Capabilities:** Great for GPUs, special toolchains, big caches, private network access, or expensive dependencies.

---

## Handy combos

- **Matrix + conditional flags**
    
    ```yaml
    - run: pytest
      if: ${{ matrix.python != '3.10' }}  # skip a step for a variant
    
    ```
    
- **Dispatch with inputs → control matrix**
    
    ```yaml
    on:
      workflow_dispatch:
        inputs:
          py:
            type: string
            default: "3.11,3.12"
    jobs:
      test:
        strategy:
          matrix:
            python: ${{ fromJSON(format('["{0}"]', replace(inputs.py, ',', '","'))) }}
    
    ```
    
- **Cron with guard**
    
    ```yaml
    jobs:
      nightly:
        if: ${{ github.event_name == 'schedule' }}
        runs-on: ubuntu-latest
        steps: …
    
    ```
    

If you tell me a concrete scenario (e.g., “nightly build on main; manual hotfix deploy with input; matrix across py/OS; keep running when one fails; publish artifacts only on success; use our GPU runner for torch tests”), I’ll wire it into a single, clean workflow.

