# Changelog

## Definition

A changelog is a file that documents notable changes, additions, bug fixes, and improvements made to a project over time. It provides a human-readable history of the project and helps users and developers understand what changed between versions. In JavaScript, changelogs are typically maintained in `CHANGELOG.md` files.

## Changelog Formats

### Keep a Changelog Format

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- New feature X
- New API endpoint for users

### Changed
- Updated login flow

### Fixed
- Fixed bug in payment processing

### Removed
- Deprecated legacy auth module

## [1.2.0] - 2026-01-15

### Added
- User profile customization
- Dark mode support

### Fixed
- Memory leak in WebSocket handler
```

## Programmatic Changelog Generation

```javascript
// Using conventional-changelog
const conventionalChangelog = require("conventional-changelog");

const options = {
  header: `# Changelog\n\n`,
  preset: "angular",
  gitRawCommitsOpts: {},
  parserOpts: {},
  writerOpts: {},
};

conventionalChangelog(options).pipe(process.stdout);
```

### Parsing Changelog Files

```javascript
const fs = require("fs");

function parseChangelog(filePath) {
  const content = fs.readFileSync(filePath, "utf-8");
  const versions = [];

  const sections = content.split(/^## \[(.+?)\]/m).slice(1);

  for (let i = 0; i < sections.length; i += 2) {
    const version = sections[i];
    const details = sections[i + 1] || "";
    versions.push({ version, details: details.trim() });
  }

  return versions;
}

const changelog = parseChangelog("CHANGELOG.md");
console.log(changelog);
```

### Generating Changelog from Git Commits

```javascript
const { execSync } = require("child_process");

function generateFromGit() {
  const log = execSync(
    'git log --pretty=format:"%h|%s|%ai" --no-merges',
    { encoding: "utf-8" }
  );

  const commits = log.split("\n").map((line) => {
    const [hash, message, date] = line.split("|");
    return { hash, message, date };
  });

  let changelog = "# Changelog\n\n";
  let currentDate = "";

  commits.forEach((commit) => {
    const date = commit.date.split(" ")[0];
    if (date !== currentDate) {
      changelog += `\n## ${date}\n\n`;
      currentDate = date;
    }
    changelog += `- ${commit.message} (${commit.hash})\n`;
  });

  return changelog;
}

console.log(generateFromGit());
```

### Semantic Versioning Helper

```javascript
function bumpVersion(version, type = "patch") {
  const [major, minor, patch] = version.split(".").map(Number);

  switch (type) {
    case "major":
      return `${major + 1}.0.0`;
    case "minor":
      return `${major}.${minor + 1}.0`;
    case "patch":
    default:
      return `${major}.${minor}.${patch + 1}`;
  }
}

console.log(bumpVersion("1.2.3", "major")); // "2.0.0"
console.log(bumpVersion("1.2.3", "minor")); // "1.3.0"
console.log(bumpVersion("1.2.3", "patch")); // "1.2.4"
```

### Automated Changelog Entry

```javascript
function createChangelogEntry(type, scope, description, breaking = false) {
  const typeMap = {
    feat: "Added",
    fix: "Fixed",
    docs: "Changed",
    style: "Changed",
    refactor: "Changed",
    perf: "Changed",
    test: "Changed",
    chore: "Changed",
    revert: "Reverted",
  };

  const category = typeMap[type] || "Changed";
  const scopeText = scope ? `**${scope}:** ` : "";
  const breakingText = breaking ? " **BREAKING CHANGE**" : "";

  return `- ${category}: ${scopeText}${description}${breakingText}`;
}

// Usage
const entry = createChangelogEntry("feat", "auth", "add OAuth2 support");
console.log(entry);
```

## Common Use Cases

- **Version tracking** — Document what changed in each release
- **Release notes** — Generate notes for GitHub/GitLab releases
- **API documentation** — Track breaking changes for consumers
- **Compliance** — Maintain audit trail for regulated projects
- **Team communication** — Keep team members informed of changes

## Common Mistakes

```javascript
// Mistake 1: Not following a consistent format
// Bad: mixing date formats, inconsistent categories
// v1.0 - Fixed stuff
// 2026/01/15 - Added thing

// Mistake 2: Forgetting to document breaking changes
// Always mark breaking changes prominently

// Mistake 3: Auto-generating without human review
// Always review generated changelogs before committing
```

## Related Topics

- [[Version-Control]]
- [[Node-JS]]
- [[NPM]]
- [[Build-Tools]]
- [[Clean-Code]]
- [[Testing]]

## Quick Revision

| Aspect | Description |
|--------|-------------|
| Purpose | Document project changes over time |
| Format | Follow "Keep a Changelog" standard |
| Categories | Added, Changed, Deprecated, Removed, Fixed, Security |
| Tooling | `conventional-changelog`, `standard-version` |
| Versioning | Use SemVer (major.minor.patch) |
| Best Practice | Keep entries concise and user-focused |
