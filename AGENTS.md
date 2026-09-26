# Firecrawl Component Guide

This is a multi-runtime monorepo. Work from the component directory, read its manifest and local README first, and do not infer one component's commands for another.

- `apps/api/` is the Node API service; use its `package.json` and `pnpm-lock.yaml` for setup and checks.
- `apps/playwright-service-ts/` and `apps/test-suite/` are separate Node services/test harnesses with their own manifests.
- `apps/js-sdk/firecrawl/` is the JavaScript SDK package with build/tests; its parent `apps/js-sdk/package.json` has only a failing placeholder test. `apps/python-sdk/` and `apps/rust-sdk/` have separate Python and Cargo workflows.
- `docker-compose.yaml` composes services and can create runtime effects; inspect it but do not use it as routine validation.

For the API, use Node and pnpm 9+ as documented in `CONTRIBUTING.md`: from `apps/api/`, run `pnpm install` and `pnpm run build`. Development uses `pnpm run start` plus `pnpm run workers` and Redis; these process crawl jobs, so use an isolated configuration and the requested scope. The API has no lint script; `pnpm run format` writes files. Its default `pnpm test` includes external integration tests and is not a safe offline gate. Inspect a selected test and `jest.setup.js` before a focused invocation such as `pnpm exec jest --runTestsByPath <reviewed-test-file> --runInBand`; use mocks/fixtures and state unmet service/config prerequisites.

For JavaScript SDK changes, use Node 22+ as required by its manifest, then run `npm install` and `npm run build` from `apps/js-sdk/firecrawl/`. Inspect its Jest cases before `npm test`, since provider-backed tests need separate configuration. From `apps/playwright-service-ts/`, use its local dependencies, `npm run build`, and `npm run dev` for a scoped service run. Other SDKs/harnesses must use their own manifests; `apps/test-suite/` contains crawl and load tests, not a default local unit gate. Complete changes with the relevant build and safely selected tests, then report executed cases and unavailable integration evidence. Do not run deployments, production evaluations, publishing workflows, or network crawling for routine validation. Preserve API and SDK compatibility boundaries, keep secrets in ignored local configuration, and start from `git status --short`.
