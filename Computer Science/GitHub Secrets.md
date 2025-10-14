#computer_science #github #git 

Here’s the essentials you should know about **GitHub Secrets** for Actions—what they are, how to use them safely, and a few pro patterns.

# What they are

- **Encrypted key–value pairs** for credentials (API tokens, DB passwords, etc.). Masked in logs.
- Scopes: **Repository secrets**, **Organization secrets** (share to many repos), and **Environment secrets** (e.g., `dev`, `staging`, `prod` with approvals & branch rules).
- Access in workflows via the **`secrets` context**.

# How to reference secrets

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: prod            # grants access to env-level secrets
    steps:
      - uses: actions/checkout@v4

      - name: Use a secret as an env var
        env:
          API_KEY: ${{ secrets.MY_API_KEY }}
        run: curl -H "Authorization: Bearer $API_KEY" <https://api.example.com>

      - name: Pass secret to an action input
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.REGISTRY_USER }}
          password: ${{ secrets.REGISTRY_PASS }}

```

# Variables vs Secrets

- **Actions Variables (`vars`)** hold non-sensitive config (regions, flags).
- **Secrets** are for sensitive values only. Prefer variables for everything else.

# GITHUB_TOKEN vs PAT

- **`GITHUB_TOKEN`** is auto-provisioned per workflow run; good for checking out code, pushing commits, creating releases, etc. Scope it with `permissions:` (least privilege).
- Use a **PAT (fine-grained)** only when `GITHUB_TOKEN` can’t access external stuff or cross-repo scopes you need.

```yaml
permissions:
  contents: write
  id-token: write   # for OIDC (see below)
  packages: write

```

# Forks & PR security (easy to get wrong)

- **Secrets are NOT available** to workflows triggered by `pull_request` **from forks**. This prevents exfiltration by untrusted code.
- If you must run privileged steps for forked PRs, use:
    - `pull_request_target` (runs on the base repo context) **but** checkout the base ref, not the attacker’s code, for any step using secrets:
        
        ```yaml
        - uses: actions/checkout@v4
          with: { ref: ${{ github.event.pull_request.base.ref }} }
        
        ```
        
    - Or split into a second workflow triggered by `workflow_run` after basic checks pass.
        
- Use **Environments** with required approvals for prod secrets.

# Writing secrets to files (common need)

```yaml
- name: Write service account
  run: |
    echo "${{ secrets.GCP_SA_JSON }}" > key.json
    chmod 600 key.json

```

# Outputs & masking gotchas

- GitHub masks exact secret values in logs, but **transforms** (e.g., base64, slices) won’t be masked. Never `echo` secrets.
- Prefer passing secrets via env/inputs; avoid printing. For debugging, print only metadata (e.g., `${#API_KEY}` length).

# Rotation & management tips

- Keep secrets **short-lived**. Rotate regularly.
- Centralize with **org secrets** when multiple repos share creds.
- Prefer **OIDC** (no static secrets) to cloud providers.

## Passwordless cloud auth via OIDC (best practice)

Example: assume an AWS IAM role without storing keys.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/ci-deploy
          aws-region: eu-west-2
      - run: aws s3 ls s3://my-bucket

```

(Equivalent actions exist for Azure and GCP.)

# Typical secrets you’ll define

- `PYPI_API_TOKEN` (publish packages)
- `REGISTRY_USER` / `REGISTRY_PASS` (GHCR/Docker Hub)
- `PROD_DATABASE_URL` (deploy)
- `SLACK_WEBHOOK_URL` (notifications)
- `CODECOV_TOKEN` (coverage upload, if required)

# Using environment secrets for gated deploys

```yaml
on:
  workflow_dispatch:
    inputs:
      version: { type: string, required: true }

jobs:
  release:
    runs-on: ubuntu-latest
    environment:
      name: production         # requires approval if configured
      url: <https://app.example.com>
    steps:
      - uses: actions/checkout@v4
      - run: python -m build
      - uses: pypa/gh-action-pypi-publish@release/v1
        with:
          password: ${{ secrets.PYPI_API_TOKEN }}

```

# Don’ts (quick checklist)

- ❌ Don’t commit secrets in repo (enable **secret scanning**).
- ❌ Don’t print secrets in logs or artifacts.
- ❌ Don’t grant secrets to forked PR runs.
- ❌ Don’t over-scope tokens; use `permissions:` to minimize.

If you tell me your target stack (PyPI, GHCR, AWS/GCP/Azure), I’ll draft a minimal secrets plan + workflows (including OIDC) tailored to your repo.