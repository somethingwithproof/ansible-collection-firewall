# CI and Sonar analysis

`CI` retains YAML lint, production-profile Ansible lint, disposable Molecule
scenarios, and collection builds. Pull-request Molecule jobs use GitHub-hosted
runners; trusted push/manual jobs retain the existing self-hosted runner labels.
Passing a collection build is packaging evidence, not firewall acceptance.

`SonarQube Cloud` supplements these checks. The sources are both roles, workflow
code, and collection metadata. Molecule files are classified as tests. Generated
tooling caches and local worktrees are excluded. No runtime coverage percentage
is supplied: this collection has no executable coverage-report pipeline.

Sonar is selective while the implementation develops. Set repository variable
`ENABLE_SONAR=true` to make default-branch pushes, same-repository PRs from
`sonar/*` or `release/*`, and requested manual runs eligible. Normal feature/fix
PRs and fork PRs skip secret-dependent analysis. An absent/false switch also
skips it. `Sonar eligibility` reports the decision; `Sonar Quality Gate` is
skipped cleanly rather than remaining pending. Requested scanner, authentication,
configuration, and quality-gate failures remain failures.

## Hosted configuration

The public project is registered in Sonar organization `somethingwithproof` as
[`somethingwithproof_ansible-collection-firewall`](https://sonarcloud.io/dashboard?id=somethingwithproof_ansible-collection-firewall).
GitHub Actions is enabled. The project is bound to this GitHub repository,
its main branch is `main`, and automatic analysis is disabled so CI owns scans.
Its configuration is:

- Secret `SONAR_TOKEN`: a credential authorized to analyze that project.
- Variable `SONAR_PROJECT_KEY`: `somethingwithproof_ansible-collection-firewall`.
- Variable `ENABLE_SONAR`: `true` when hosted analysis is ready.

The organization and Cloud host reuse the verified organization configuration;
the repository variable supplies the registered project key. Keep credentials out of commits.
The dedicated CI credential expires on **2027-01-05** and must be rotated before
then. Cloud user tokens inherit the issuing account’s permissions; a separately
named token permits independent revocation but does not provide project-only scope.
Avoid simultaneous automatic and CI-based analysis for the same project.

The first hosted analysis on 2026-10-07 completed and reported seven workflow
findings. Its new-project quality gate was not computed (`NONE`), which the
scanner correctly treated as a failed requested gate; this is not a passing
result. After configuring a 30-day new-code period, the
[second hosted run](https://github.com/somethingwithproof/ansible-collection-firewall/actions/runs/37591756162)
passed. The seven existing findings remain visible: a new-code gate passing
does not mean the entire repository has no findings. Production-profile Ansible lint remains enforced. Public defaults live in
the canonical `firewall` role, and `policy` imports it to preserve the existing
entry point without suppressing the role-prefix rule. Collection runtime
metadata declares the tested ansible-core 2.19 baseline. Privileged Molecule acceptance requires the intentionally
selected disposable environment already documented in AGENTS.md.

After merging this workflow, use GitHub's Actions page or:

```sh
gh workflow run sonar.yml --repo somethingwithproof/ansible-collection-firewall --ref main -f run_sonar=true
```

To analyze an existing PR manually, select its current same-repository head
branch and supply its number. The workflow verifies its head SHA and rejects
forks. Explicit branch/PR properties prevent manual branch scans from replacing
main’s results:

```sh
gh workflow run sonar.yml --repo somethingwithproof/ansible-collection-firewall \
  --ref fix/sonar-workflow-findings -f run_sonar=true -f pull_request_number=20
```

Manual branch analysis without a PR number requires the provider’s branch-analysis
support; unsupported branches fail visibly. Main and PR scans retain distinct
identities.

Current merge policy should prioritize `Lint`, `Molecule`, and collection
validation. Keep Sonar optional until the backlog and provider configuration
are verified. Later, expand its eligibility to all trusted PRs and add the exact
check **Sonar Quality Gate** to the default-branch ruleset alongside correctness
checks. The workflow cannot and does not alter branch protection.

## Reproducible tooling

CI uses Python 3.11 and separate hash-locked, wheel-only dependency sets for
lint, Molecule, and packaging. Setup-python caches dependency downloads against
each lock; installation still verifies hashes. Dependabot merge automation runs
from trusted workflow-completion, scheduled, and manual events, reads PR/check
metadata only, and matches the checked head commit when merging. It never
checks out or executes a PR’s code with write credentials.

Regenerate locks using the repository’s runtime selector and a Linux target:

```sh
for task in ci-lint ci-molecule ci-build; do
  mise exec uv@0.10.6 -- uv pip compile "requirements/$task.in" \
    --python-version 3.11 --python-platform x86_64-manylinux_2_35 \
    --only-binary :all: --generate-hashes \
    --output-file "requirements/$task.txt"
done
```

Review dependency updates and validate wheel installation and existing checks.
Do not relax production-profile lint or remove Molecule acceptance failures to
obtain green CI. PR runs cancel obsolete validation; trusted release runs are
not cancelled by that policy.

Molecule 26 runs one selected distro per matrix job. Its obsolete `lint`
subcommand was removed from the scenario sequence because the required CI
`Lint` job already runs YAML and production-profile Ansible lint before
Molecule. Staging and cleanup are temporary work; persistent configuration
promotion and ruleset application occur only when the validated checksum changes.
Backup remains before promotion, and rollback uses a backup only when enabled
and a prior configuration exists.
