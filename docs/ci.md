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

The audit found no matching Firewall project in Sonar organization
`somethingwithproof`. Register this public repository there, then configure:

- Secret `SONAR_TOKEN`: a credential authorized to analyze that project.
- Variable `SONAR_PROJECT_KEY`: the actual key returned by Sonar project setup.
- Variable `ENABLE_SONAR`: `true` when hosted analysis is ready.

The organization and Cloud host reuse the verified organization configuration;
the project key is deliberately not guessed. Keep credentials out of commits.
Avoid simultaneous automatic and CI-based analysis for the same project.

GitHub Actions is currently disabled for the repository. Project registration,
credential setup, and authorized Actions enablement are still required before a
hosted run can be claimed. Existing production-profile Ansible lint findings
must be repaired; the Sonar workflow does not relax that profile or turn its
failures into passes. Privileged Molecule acceptance requires the intentionally
selected disposable environment already documented in AGENTS.md.

After merging this workflow, use GitHub's Actions page or:

```sh
gh workflow run sonar.yml --repo somethingwithproof/ansible-collection-firewall --ref main -f run_sonar=true
```

Current merge policy should prioritize `Lint`, `Molecule`, and collection
validation. Keep Sonar optional until the backlog and provider configuration
are verified. Later, expand its eligibility to all trusted PRs and add the exact
check **Sonar Quality Gate** to the default-branch ruleset alongside correctness
checks. The workflow cannot and does not alter branch protection.
