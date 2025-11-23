# NoTube Proto

This repository contains the Protocol Buffer definitions and build pipeline for the **NoTube** project's data models. It serves as the single source of truth for the schema (specifically "Alternatives") and automatically generates and publishes client libraries for JavaScript and Dart.

## Structure

*   **`src/`**: Contains the Protocol Buffer definitions (e.g., `alternative.proto`).
*   **`data/`**: Contains static data files (e.g., `default_alternatives.json`) that are bundled with the generated libraries.
*   **`templates/`**: Template files (`package.json`, `pubspec.yaml`) used during the build process to package the generated code.
*   **`buf.yaml`**: Configuration for [Buf](https://buf.build/), the tool used for linting and code generation.

## Build & Release Process

This repository uses **GitHub Actions** to automate the entire release workflow. On every push to the `main` branch, the following steps occur:

1.  **Versioning**: The project version is automatically bumped based on the commit messages (using `mathieudutour/github-tag-action`).
2.  **Code Generation**: `buf generate` runs to produce JavaScript and Dart source code from the `.proto` files.
3.  **Packaging & Publishing**:
    *   **JavaScript**: The generated JS code and `data/` files are packaged and pushed to the **`builds/js`** branch.
    *   **Dart**: The generated Dart code and `data/` files are packaged and pushed to the **`builds/dart`** branch.
4.  **Release**: A formal GitHub Release is created with a changelog derived from the commits.

## Bumping

The action will parse the new commits since the last tag using the semantic-release conventions.

[semantic-release](https://github.com/semantic-release/semantic-release) uses the commit messages to determine the type of changes in the codebase. Following formalized conventions for commit messages, semantic-release automatically determines the next semantic version number.

By default semantic-release uses [Angular Commit Message Conventions](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines).

Here is an example of the release type that will be done based on a commit messages:

| Commit message | Release type |
| --- | --- |
| `fix(pencil): stop graphite breaking when too much pressure applied` | **Patch Release** |
| `feat(pencil): add 'graphiteWidth' option` | **Minor Release** |
| `perf(pencil): remove graphiteWidth option` <br><br> `BREAKING CHANGE: The graphiteWidth option has been removed. The default graphite width of 10mm is always used for performance reasons.` | **Major Release** |

If no commit message contains any information, then `default_bump` will be used.
