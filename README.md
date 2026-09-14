# Secrets/Vulnerability Pipeline
Created a GitHub CI/CD pipeline that automatically checks code for secrets (ex. API keys or Tokens) that may have been left in there, using Gitleaks.

## What it does
Developers on occasion commit real credentials directly into their source code by accident. Once the code is pushed it becomes part of permanent git history, even if they try to delete it later on. This pipeline catches the mistake right after it's pushed, before the code ever gets merged or deployed, by checking each push for known credential patterns and failing the build if one is found.

## How does it work?
- Triggered every time a push or PR is made to the main branch
- Runs Gitleaks, which matches against known secret formats
- If something is found, it fails the pipeline and shows the exact file/line where a secret was found

## Proof of it working
[Link to Clean Run](https://github.com/maheemkhan070/secrets-vulnerability-pipeline/actions/runs/34800151140) - no secrets were present
[Link to Failed Run](https://github.com/maheemkhan070/secrets-vulnerability-pipeline/actions/runs/34799775824) - the test AWS key was flagged (rule: `aws-access-token`) at app.py line 8

## Possible Limitations
Pattern-based detection can only catch known credential formats, so it won't flag a generic secret such as a custom password since it doesn't recognize its structure.
