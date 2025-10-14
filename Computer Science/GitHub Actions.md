#computer_science #github #git 

Absolutely—here’s a curated, battle-tested set of **useful GitHub Actions** (with when to use them, gotchas, and tiny snippets). I grouped them by job-to-be-done so you can copy/paste fast.

# Test & Coverage

- **codecov/codecov-action@v4** – upload coverage to Codecov.
    
    ```yaml
    - run: pytest --cov=your_pkg --cov-report=xml
    - uses: codecov/codecov-action@v4
      with:
        token: ${{ secrets.CODECOV_TOKEN }}   # not needed for public repos
    
    ```
    
- **actions/upload-artifact@v4** / **download-artifact@v4** – move reports/artifacts across jobs.
    
    ```yaml
    - uses: actions/upload-artifact@v4
      with: { name: coverage, path: coverage.xml }
    
    ```
    

# Lint/Format/Static analysis

- **astral-sh/ruff-action@v3** – ultra-fast lint.
    
- **psf/black@stable** (or run via `pip`) – formatting.
    
- **mypy** (run via `pip`) – static typing.
    
- **pre-commit/action@v3.0.1** – enforce your `.pre-commit-config.yaml`.
    
    ```yaml
    - uses: pre-commit/action@v3.0.1
    
    ```
    

# Commit / Push / PR automation

- **ad-m/github-push-action@v0.8.0** – push changes from CI (docs, version bumps, lockfiles).
    
    ```yaml
    - uses: ad-m/github-push-action@v0.8.0
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        branch: ${{ github.ref_name }}
    
    ```
    
- **peter-evans/create-pull-request@v6** – open a PR from CI (e.g., bump deps).
    
    ```yaml
    - uses: peter-evans/create-pull-request@v6
      with:
        commit-message: "chore: update deps"
        title: "chore: update deps"
        branch: ci/update-deps
    
    ```
    
- **actions/labeler@v5** – auto-label PRs based on file paths.
    

# SSH / Remote execution

- **appleboy/ssh-action@v1.0.3** – run commands on a remote host via SSH (simple).
    
    ```yaml
    - uses: appleboy/ssh-action@v1.0.3
      with:
        host: ${{ secrets.SSH_HOST }}
        username: ${{ secrets.SSH_USER }}
        key: ${{ secrets.SSH_KEY }}         # private key
        script: |
          cd /srv/app && docker compose pull && docker compose up -d
    
    ```
    
    ⚠️ Use **deployment keys**/least privilege; never echo secrets. For complex workflows, consider **GitHub Environments** with approvals.
    

# Releases & Packaging

- **pypa/gh-action-pypi-publish@release/v1** – publish to PyPI.
    
    ```yaml
    - run: python -m build
    - uses: pypa/gh-action-pypi-publish@release/v1
      with: { password: ${{ secrets.PYPI_API_TOKEN }} }
    
    ```
    
- **softprops/action-gh-release@v2** – create GitHub Releases, attach artifacts.
    
    ```yaml
    - uses: softprops/action-gh-release@v2
      with: { files: "dist/*" }
    
    ```
    
- **python-semantic-release/python-semantic-release@v9** – auto versioning & changelog from Conventional Commits.
    

# Containers & Registries

- **docker/setup-buildx-action@v3** + **docker/login-action@v3** + **docker/build-push-action@v6**
    
    ```yaml
    - uses: docker/login-action@v3
      with: { registry: ghcr.io, username: ${{ github.actor }}, password: ${{ secrets.GITHUB_TOKEN }} }
    - uses: docker/build-push-action@v6
      with:
        push: true
        tags: ghcr.io/OWNER/APP:${{ github.sha }}
    
    ```
    

# Caching & Speed

- **actions/setup-python@v5** with `cache: pip` – built-in pip cache.
    
- **actions/cache@v4** – custom caches (e.g., `.mypy_cache`, `.pytest_cache`, `~/.cache/pip`, compiled datasets).
    
    ```yaml
    - uses: actions/cache@v4
      with:
        path: ~/.cache/pip
        key: pip-${{ runner.os }}-${{ hashFiles('**/requirements*.txt') }}
    
    ```
    

# Security & Scanning

- **github/codeql-action@v3** – SAST code scanning.
- **actions/dependency-review-action@v4** – block PRs that add vulnerable deps.
- **trufflesecurity/trufflehog@v3** – scan for secrets in history & diffs.

# Docs & Pages

- **actions/configure-pages@v5** + **actions/upload-pages-artifact@v3** + **actions/deploy-pages@v4** – publish docs to Pages.
    
    ```yaml
    - run: mkdocs build
    - uses: actions/upload-pages-artifact@v3
    - uses: actions/deploy-pages@v4
    
    ```
    

# Cloud Deploy (passwordless best practice)

- **aws-actions/configure-aws-credentials@v4** / **azure/login@v2** / **google-github-actions/auth@v2** – OIDC-based auth (no static keys).
    
    ```yaml
    permissions: { id-token: write, contents: read }
    - uses: aws-actions/configure-aws-credentials@v4
      with:
        role-to-assume: arn:aws:iam::<acct>:role/ci-deploy
        aws-region: eu-west-2
    
    ```
    

# Monorepo / Selective CI

- **dorny/paths-filter@v3** – run jobs only when certain paths change.
    
    ```yaml
    - uses: dorny/paths-filter@v3
      id: filter
      with:
        filters: |
          backend: { paths: ['backend/**'] }
          frontend: { paths: ['frontend/**'] }
    - if: ${{ steps.filter.outputs.backend == 'true' }}
      run: make test-backend
    
    ```
    

# Notifications

- **slackapi/slack-github-action@v1** – post to Slack (use a webhook/secret).
- **rtCamp/action-slack-notify@v2** – simple alternative.

# QA niceties

- **actions/stale@v9** – auto-mark/close inactive issues/PRs.
- **danger/danger-js@11** – custom PR comments (summarize lint, changelog, etc.).
- **yaananth/run-matrix@v3** – generate dynamic matrices at runtime.

---

## Suggested “starter pack” for a Python repo

1. **CI**: checkout, setup-python (with cache), ruff, pytest (+coverage), upload artifact
2. **Coverage**: codecov
3. **Quality**: pre-commit/action (black, isort, ruff, mypy)
4. **Release**: python-semantic-release → gh-release → PyPI (if library)
5. **Container**: build-push to GHCR (if service)
6. **Docs**: build + deploy Pages
7. **Security**: dependency-review + CodeQL

If you tell me your repo type (library vs web service), where you deploy (GHCR, AWS/GCP/Azure), and whether you want nightly jobs or fork-safe PRs, I’ll stitch these into a single clean workflow file (or reusable workflows) tailored to you.