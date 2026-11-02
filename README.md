# Eve

Multi-model AI pull request review, as a reusable GitHub Actions workflow. Eight NVIDIA-hosted models review a pull request's diff, a
ninth consolidates them, and every blocker and warning is checked against the code before it is shown.

**The diff, title and description of each reviewed pull request are sent to NVIDIA.** Do not enable Eve on a repository whose code
must not leave your control.

## Turn it on in a repository

1. Copy [`examples/eve.yml`](examples/eve.yml) to `.github/workflows/eve.yml`.
2. Add the repository secret `NVIDIA_API_KEY`.
3. Optional: `.github/eve-context.md` on the default branch holds facts the code does not show (see
   [the example](examples/eve-context.example.md)); the variable `AI_REVIEW_ENABLED=false` turns Eve off; `AI_REVIEW_APP_ID` plus the
   secret `AI_REVIEW_APP_PRIVATE_KEY` make it comment as a GitHub App instead of `github-actions[bot]`.

Pin the `uses:` line to a release tag or commit once there is one: the workflow runs with your secrets.

## Advisory only

Eve is a reviewer, not a security gate. Do not make "Eve - Go/No-Go Decision" a required check: a failed or partial review ends
neutral, never as a failure, and a pull request's text can try to talk the models into dismissing a finding. A person still decides
what merges.

## What it does and does not do

See the header of [`.github/workflows/review.yml`](.github/workflows/review.yml). It never checks out a pull request's code (it reads
the diff through the API), because the calling workflow runs on `pull_request_target` and holds secrets. Keep it that way.

## Tests

`python3 eval/test_eve.py` tests the checking code with a fake model. See [`eval/README.md`](eval/README.md).
