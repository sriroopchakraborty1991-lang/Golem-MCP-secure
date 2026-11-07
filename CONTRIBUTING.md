# Contributing to Golem

Thanks for helping make MCP servers safer! This guide explains how to set up the project, make a change and get it merged.

## Table of Contents

- [Ways to contribute](#ways-to-contribute)
- [Development setup](#development-setup)
- [Project layout](#project-layout)
- [Making a change](#making-a-change)
- [Adding a scanner rule](#adding-a-scanner-rule)
- [Code style](#code-style)
- [Commit messages](#commit-messages)
- [Pull requests](#pull-requests)
- [Reporting security issues](#reporting-security-issues)
- [License of contributions](#license-of-contributions)

## Ways to contribute

- 🐛 **Report a bug** — use the *Bug report* issue template.
- 💡 **Suggest a feature** — use the *Feature request* template.
- 🎯 **Report a scanner false positive / missed risk** — use the *Scanner finding* template.
- 📝 **Improve docs** — typos, clearer examples and diagrams are always welcome.
- 🔧 **Send code** — pick an open issue or propose one first for larger changes.

## Development setup

Requirements: **Node.js ≥ 18**, npm, and **Docker** (for sandboxes and Docker Compose).

```bash
git clone https://github.com/sriroopchakraborty1991-lang/Golem-MCP-secure.git
cd Golem-MCP-secure
npm install

cp packages/backend/.env.example packages/backend/.env
cp packages/frontend/.env.example packages/frontend/.env
# set JWT_SECRET (min. 16 characters) in packages/backend/.env

npm run dev:backend    # terminal 1 — http://localhost:3001/health
npm run dev:frontend   # terminal 2 — http://localhost:3000
```

Or run the whole stack with `npm run dev` (Docker Compose).

## Project layout

| Path | What lives there |
|---|---|
| `packages/backend/src/routes.ts` | All `/api` routes (Fastify) |
| `packages/backend/src/mcp/scanner.ts` | Server, static and drift checks |
| `packages/backend/src/sandbox/runner.ts` | Docker sandbox runner |
| `packages/backend/src/threat/scoring.ts` | Deterministic threat scoring |
| `packages/backend/src/db/` | SQLite data access |
| `packages/backend/tests/` | `node:test` suites |
| `packages/frontend/src/pages/` | React pages |

## Making a change

1. Fork the repo and create a branch from `sri-rIN7eT`:
   ```bash
   git checkout -b feat/short-description
   ```
2. Keep the change focused — one fix or feature per pull request.
3. Run the checks before pushing:
   ```bash
   npm run lint
   npm run format
   npm test
   ```
4. Update `README.md` if you change behaviour, configuration or the API.

## Adding a scanner rule

Golem's checks are intentionally **deterministic and explainable**. A new rule should:

- live in `packages/backend/src/mcp/scanner.ts` (`runServerChecks`, `runStaticChecks` or `runDriftChecks`);
- return a finding with a clear `severity` (`high` / `medium` / `low`), `category`, `title`, `evidence` and an actionable `recommendation`;
- avoid network calls, ML models or anything that makes results hard to reproduce;
- come with tests in `packages/backend/tests/mcp.test.ts` covering both a positive and a negative case;
- be added to the **Implemented Security Checks** table in `README.md`.

## Code style

- TypeScript everywhere; prefer explicit types on exported functions.
- Formatting is handled by **Prettier** and linting by **ESLint** (`.prettierrc.json`, `.eslintrc.json`).
- Keep functions small and avoid new dependencies unless they are clearly needed.
- Never commit secrets, `.env` files or real tokens in test fixtures.

## Commit messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add SARIF export for scan findings
fix: handle empty inputSchema in static checks
docs: clarify Docker socket setup
test: cover drift detection for removed tools
```

## Pull requests

- Fill in a short description: **what** changed and **why**.
- Link the related issue (`Closes #123`).
- Include screenshots for UI changes.
- Make sure `npm run lint` and `npm test` pass.
- Be ready for review feedback — small, focused PRs get merged fastest.

## Reporting security issues

Please read [SECURITY.md](SECURITY.md). Do **not** include secrets, tokens or private endpoints in public issues.

## License of contributions

Golem is dual-licensed under **Apache-2.0 OR GPL-3.0** (see [LICENSE](LICENSE)). By submitting a contribution you agree that it is licensed under the same terms.
