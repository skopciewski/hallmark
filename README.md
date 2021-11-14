# hallmark

> **Markdown Style Guide, with linter and automatic fixer.** :sparkles:  
> Powered by [`remark`][remark].

[![npm status](http://img.shields.io/npm/v/hallmark.svg)](https://www.npmjs.org/package/hallmark)
[![node](https://img.shields.io/node/v/hallmark.svg)](https://www.npmjs.org/package/hallmark)
[![Test](https://img.shields.io/github/workflow/status/vweevers/hallmark/Test?label=test)](https://github.com/vweevers/hallmark/actions/workflows/test.yml)
[![JavaScript Style Guide](https://img.shields.io/badge/standard-informational?logo=javascript&logoColor=fff)](https://standardjs.com)
[![Markdown Style Guide](https://img.shields.io/badge/hallmark-informational?logo=markdown)](https://github.com/vweevers/hallmark)
[![Common Changelog](https://common-changelog.org/badge.svg)](https://common-changelog.org)

## Table of Contents

<details><summary>Click to expand</summary>

- [Why](#why)
- [Quick Start](#quick-start)
- [What You Might Do](#what-you-might-do)
- [Requirements](#requirements)
- [Rules](#rules)
- [Usage](#usage)
  - [Commands](#commands)
    - [`lint`](#lint)
    - [`fix`](#fix)
    - [`cc add <target>`](#cc-add-target)
- [Package Options](#package-options)
  - [`ignore`](#ignore)
  - [`autolinkReferences`](#autolinkreferences)
  - [`changelog`](#changelog)
  - [`validateLinks`](#validatelinks)
  - [`paddedTable`](#paddedtable)
  - [`toc`](#toc)
  - [`plugins`](#plugins)
  - [`fixers`](#fixers)
- [Opt-in Features](#opt-in-features)
  - [Table of Contents](#table-of-contents-1)
- [Reporters](#reporters)
- [Install](#install)
- [License](#license)

</details>

## Why

This module saves you time in three ways:

- **No configuration.** The easiest way to enforce markdown code quality in your project. No decisions to make. No `remark` plugins to manage.
- **Automatically format markdown.** Run `hallmark fix` to format markdown, wrap GitHub issues and usernames in links, autocomplete a `CHANGELOG.md` following [Common Changelog](https://common-changelog.org) and more.
- **Catch style issues & mistakes early.** Save code review time by eliminating back-and-forth between reviewer & contributor.

## Quick Start

Lint `*.md` files:

```
hallmark
```

Fix markdown files in place:

```
hallmark fix
```

Fix custom files:

```
hallmark fix CHANGELOG.md docs/*.md
```

Add new minor version or existing version to changelog, optionally without content:

```
hallmark cc add minor
hallmark cc add 4.2.0 --no-commits
```

## What You Might Do

Add `hallmark` to your `package.json`:

```json
{
  "name": "my-awesome-package",
  "devDependencies": {
    "hallmark": "^2.0.0"
  },
  "scripts": {
    "test": "hallmark && node my-tests.js"
  }
}
```

Markdown is then checked automatically when you run `npm test`:

```
$ npm test
README.md:5:3
  ⚠️  5:3  Found reference to undefined definition  remark-lint:no-undefined-references

  1 warning
```

## Requirements

- The working directory must be a git repository
- It must either contain a `package.json` with a [`repository`](https://docs.npmjs.com/files/package.json#repository) property, or have a git `origin` remote

## Rules

- [Use `-` as marker for bullets of items in unordered lists](https://www.npmjs.com/package/remark-lint-unordered-list-marker-style) (`- item`)
- [Use `_` as marker for emphasis](https://www.npmjs.com/package/remark-lint-emphasis-marker) (`_emphasis_`)
- [Use `*` as marker for strong](https://www.npmjs.com/package/remark-lint-strong-marker) (`**strong**`)
- [Use `x` as marker for checkboxes](https://www.npmjs.com/package/remark-lint-checkbox-character-style) (`- [x] item`)
- [Use backtick as marker for fenced code block](https://www.npmjs.com/package/remark-lint-fenced-code-marker)
- [Always use fences for code blocks](https://www.npmjs.com/package/remark-lint-code-block-style)
- [Use `---` for thematic breaks](https://www.npmjs.com/package/remark-lint-rule-style)
- [Insert links to GitHub issues, PRs and usernames](https://www.npmjs.com/package/remark-github) (not linted)
- [Collapse a Table of Contents if it exists](https://www.npmjs.com/package/remark-collapse) (not linted)
- [End file with newline](https://www.npmjs.com/package/remark-lint-final-newline)
- No dead links, references and definitions:
  - [No dead internal links](https://www.npmjs.com/package/remark-validate-links)
  - [No references to undefined definitions](https://www.npmjs.com/package/remark-lint-no-undefined-references)
  - [No unused definitions](https://www.npmjs.com/package/remark-lint-no-unused-definitions)
  - [No duplicate definitions](https://www.npmjs.com/package/remark-lint-no-duplicate-definitions)
  - [Definition labels must be lowercase](https://www.npmjs.com/package/remark-lint-definition-case)
- No more spaces than needed:
  - [Indent list items with a single space (`-  item`)](https://www.npmjs.com/package/remark-lint-list-item-indent)
  - [Indent blockquote content with a single space](https://www.npmjs.com/package/remark-lint-blockquote-indentation)
  - [Checkboxes must be followed by a single space](https://www.npmjs.com/package/remark-lint-checkbox-content-indent)
  - [No indentation before list item bullets](https://www.npmjs.com/package/remark-lint-list-item-bullet-indent)
  - [No unneeded padding around heading content](https://www.npmjs.com/package/remark-lint-no-heading-content-indent)
  - [No more than two spaces to create a hard break](https://www.npmjs.com/package/remark-lint-hard-break-spaces)
  - [No inline padding between markers and content (`_ content _`)](https://www.npmjs.com/package/remark-lint-no-inline-padding)
- Parsable literal URLs:
  - [No literal URLs without angle-brackets](https://www.npmjs.com/package/remark-lint-no-literal-urls)
  - [No angle-bracketed links (`<url>`) without protocol](https://www.npmjs.com/package/remark-lint-no-auto-link-without-protocol)
- [No blank lines without markers (`>`) in a blockquote](https://www.npmjs.com/package/remark-lint-no-blockquote-without-marker)
- Tables:
  - [Table cells must be padded](https://www.npmjs.com/package/remark-lint-table-cell-padding) ([#16](https://github.com/vweevers/hallmark/issues/16))
  - [Table rows must be fenced with pipes](https://www.npmjs.com/package/remark-lint-table-pipes)

## Usage

`hallmark [command] [options] [pattern ...]`

Lint or fix files in the current working directory. The default command is `lint`.

By default `hallmark` includes files matching `*.md`. Pass one or more glob patterns to override this. Files matching `.gitignore` patterns are ignored. To ignore additional files, use the `--ignore / -i` option.

Options:

- `--ignore / -i <pattern>`: files to ignore. Repeat to specify multiple patterns (e.g. `-i a.md -i docs/*.md`). Can also be configured via [Package Options](#package-options).
- `--help`: print usage and exit
- `--version`: print version and exit
- `--report <reporter>`: see [Reporters](#reporters)
- `--[no-]color`: force color in report (detected by default)
- `--fix`: backwards-compatible alias for fix command

### Commands

#### `lint`

Lint markdown files.

#### `fix`

Fix markdown files in place.

#### `cc add <target>`

Add a release to `CHANGELOG.md` and populate it with commits. The `target` must be one of:

- A release type: `major`, `minor`, `patch`, `premajor`, `preminor`, `prepatch`, `prerelease`
  - These take the current version from the semver-latest tag, release or `package.json` (whichever is greatest if found) and bump it
  - The `major` type bumps the major version (for example `2.4.1 => 3.0.0`); `minor` and `patch` work the same way.
  - The `premajor` type bumps the version up to the next major version and down to a prerelease of that major version; `preminor` and `prepatch` work the same way.
  - The `prerelease` type works the same as `prepatch` if the current version is a non-prerelease. If the current is already a prerelease then it's simply incremented (for example `4.0.0-rc.2` to `4.0.0-rc.3`).
- A [semver-valid](https://semver.org/) version like 2.4.0.

If the (resulting) version is greater than the current version then commits will be taken from the semver-latest tag until HEAD. I.e. documenting a new release before it's git-tagged. If the version matches an existing tag then a release will be inserted at the appriopriate place, populated with commits between that version's tag and the one before it. I.e. documenting a past release after it's git-tagged.

Additional options for this command:

- `--no-commits`: create an empty release.

Works best on a linear git history. If `hallmark` encounters other tags in the commit range (which may happen if releases were made in parallel on other branches) it will stop there and not include further (older) commits.

The `cc add` command also fixes markdown - both existing content and generated content. After you tweak the release following [Common Changelog](https://common-changelog.org) you may want to run `hallmark fix` again.

## Package Options

You can add a `hallmark` object to your `package.json` with additional configuration. For example:

```json
{
  "name": "my-awesome-package",
  "hallmark": {
    "ignore": [
      "CONTRIBUTING.md"
    ]
  }
}
```

Alternatively, for use in non-node projects, place a `.hallmarkrc` file in the working directory or any of its parent directories:

```json
{
  "ignore": [
    "CONTRIBUTING.md"
  ]
}
```

### `ignore`

A string or array of files to ignore. Merged with `--ignore / -i` if any.

### `autolinkReferences`

Autolink custom references [like GitHub Pro](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/configuring-autolinks-to-reference-external-resources) does. Must be an object with a `prefix` and `url` (if `autolinkReferences` is not set, this feature does nothing). For example, given:

```json
{
  "autolinkReferences": {
    "prefix": "JIRA-",
    "url": "https://example.atlassian.net/browse/JIRA-<num>"
  }
}
```

Then `hallmark fix` will transform:

```md
### Fixed

- Prevent infinite loop (JIRA-4275)
```

To:

```md
### Fixed

- Prevent infinite loop ([JIRA-4275](https://example.atlassian.net/browse/JIRA-4275))
```

While `hallmark lint` will warn about unlinked references.

### `changelog`

An object containing options to be passed to [`remark-common-changelog`](https://github.com/vweevers/remark-common-changelog):

- `submodules` (boolean): enable experimental git submodule support. Will (upon encountering new or empty changelog entries) collect commits from submodules and list them in the changelog as `<submodule>: <message>`.

### `validateLinks`

Boolean. Set to `false` to skip validating links. Useful when a markdown file uses HTML anchors, which not are not recognized as links. A temporary option until we decide whether to allow and parse those.

### `paddedTable`

Boolean. Set to `false` to keep markdown tables compact. A temporary option until we decide on and are able to lint a style ([`3210a96`](https://github.com/vweevers/hallmark/commit/3210a96)).

### `toc`

Boolean. Set to `false` to skip generating (or replacing) a Table of Contents. A temporary option until we write a more flexible plugin ([#36](https://github.com/vweevers/hallmark/issues/36)).

### `plugins`

An array of extra plugins, to be applied in both lint and fix mode.

### `fixers`

An array of extra plugins, to be applied in fix mode.

## Opt-in Features

### Table of Contents

_Note: this feature is likely to change ([#36](https://github.com/vweevers/hallmark/issues/36))._

Add this heading to a markdown file:

```markdown
## Table of Contents
```

Running `hallmark fix` will then create or update a table of contents.

## Reporters

The default reporter is [`vfile-reporter-shiny`](https://github.com/vweevers/vfile-reporter-shiny). Various other reporters are available:

- [`vfile-reporter`](https://npmjs.org/package/vfile-reporter)
- [`vfile-reporter-json`](https://npmjs.org/package/vfile-reporter-json)
- [`vfile-reporter-pretty`](https://npmjs.org/package/vfile-reporter-pretty)

To use a custom reporter first install it with [npm](https://npmjs.org):

```
npm i vfile-reporter-json --save-dev
```

Then pass it to `hallmark` with or without options:

```
hallmark --report json
hallmark --report [ json --pretty ]
```

In the programmatic API of `hallmark` (which is not documented yet) the reporter can also be disabled by passing `{ report: false }` as the options.

## Install

With [npm](https://npmjs.org) do:

```
npm install hallmark --save-dev
```

## License

[GPL-3.0](LICENSE) © 2018-present Vincent Weevers.

[remark]: https://www.npmjs.org/package/remark
