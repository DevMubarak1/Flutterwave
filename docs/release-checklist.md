# Release Checklist for DevMubarak1/Flutterwave

This checklist is tailored to this repository and release flow.

## 1. Repository and Metadata

- Confirm repository is public at https://github.com/DevMubarak1/Flutterwave.
- Confirm package metadata in package.json:
  - name: flutterwave-skills
  - repository.url: git+https://github.com/DevMubarak1/Flutterwave.git
  - bugs.url: https://github.com/DevMubarak1/Flutterwave/issues
  - homepage: https://github.com/DevMubarak1/Flutterwave#readme
- Confirm all skill markdown files are present in skills/*/SKILL.md.

## 2. Local Validation

- Run `npm run check`.
- Confirm no markdown rendering issues in README and docs.
- Confirm manifest list in skills/manifest.json matches actual skill folders.

## 3. skills.sh Registration

- Sign in at skills.sh.
- Connect GitHub account.
- Register repository: DevMubarak1/Flutterwave.
- Confirm skills index detects skills/*/SKILL.md.
- Test install from a clean project:
  - `npx skills add DevMubarak1/Flutterwave`

### Skills.sh CLI Quick Use

- Browse registry: https://skills.sh/
- Add by skill name: `npx skills add <skill-name>`
- Add from GitHub/GitLab repo: `npx skills add owner/repo`
- List installed skills: `npx skills list`

## 4. npm Publishing

- Ensure npm account has publish rights for package name `flutterwave-skills`.
- Authenticate with npm (`npm login` or `NPM_TOKEN` in environment).
- Publish with `npm publish --access public`.
- Tag a release in GitHub matching package version.

### Security Note

- Never commit `.npmrc` with auth tokens.
- Rotate npm access tokens if they were exposed in logs or chat history.

## 5. Post-Publish Verification

- Re-run install test:
  - `npx skills add DevMubarak1/Flutterwave`
- Verify package metadata on npm and README links.
- Create a changelog entry for the release.

## 6. Recommended Release Cadence

- Patch: endpoint wording fixes, typo corrections.
- Minor: new skills, major docs additions, expanded country guidance.
- Major: broad template/schema changes across all skills.
