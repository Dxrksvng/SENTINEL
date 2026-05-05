# Contributing

External code contributions are **not currently being accepted** while SENTINEL is in pre-alpha and going through customer validation.

If you found this repo and want to contribute anyway, the most valuable thing you can do is share what your team's AI compliance pain looks like.

- Open a GitHub Discussion (when enabled), or
- Reach the maintainer through the GitHub profile.

That signal directly shapes which problems SENTINEL solves first.

---

## What this public repo accepts

- Documentation corrections (typo, broken link, factual error in a framework reference).
- Suggestions on the public architecture overview, vision, or roadmap.
- Reports of dead reference links.

Submit those as pull requests against the documentation files in this repository.

## What this public repo does not accept

- Implementation patches — the implementation lives in a private repository during pre-alpha.
- Feature requests submitted as code — please open a Discussion describing the use case instead.

## Project conventions (for the maintainer and future contributors)

- **Conventional Commits** — `feat:`, `fix:`, `chore:`, `docs:`, `refactor:`, `test:`, `perf:`, `ci:`.
- **Honesty over hype** — no marketing language in code comments, commit messages, or changelogs.
- **Calibrated claims** — every release ships with a clear statement of what it can and cannot do.
- **No fake PII in tests, ever** — use a faker library or synthetic deterministic values.

## Security issues

See [`SECURITY.md`](./SECURITY.md). Do **not** open a public issue for a security-sensitive problem.
