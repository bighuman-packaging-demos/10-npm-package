# 10-npm-package

- `dependency`
    - repo: https://github.com/bighuman-packaging-demos/10-npm-package--dependency
    - package: https://github.com/bighuman-packaging-demos/10-npm-package--dependency/packages/1375896
- `dependent`
    - repo: https://github.com/bighuman-packaging-demos/10-npm-package--dependent
    - package: https://github.com/bighuman-packaging-demos/10-npm-package--dependent/packages/1375907

This is an example of how copy-pasting code into a repo would work. Consider this scenario: a package `dependency` we would like `dependent` to depend on is published to an npm repository. We simply `npm install dependency` to inform `npm` of the dependency.

```
~
├── dependency
│   ├── index.js
│   ├── package-lock.json
│   └── package.json
└── dependent
    ├── index.js
    ├── node_modules
    │   └── dependency
    │       ├── index.js
    │       ├── package-lock.json
    │       └── package.json
    ├── package-lock.json
    ├── package.json
    └── packages
```

All existing `npm` tooling works when `dependent` needs to receive updates from `dependency`. For example:

- `npm upgrade`
- `npx npm-check-updates`
- `"${EDITOR}" package.json && npm install`

## Publishing packages

In this demo, `dependency` and `dependent` are both packages published to [GitHub Packages](https://github.com/features/packages), which functions as an `npm` registry (among other things). To set this up, each package needs an `.npmrc` with the following contents:

```npmrc
@SCOPE:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=${GITHUB_NPM_TOKEN}
```

- `SCOPE` should be replaced with the GitHub organization's slug
- `${GITHUB_NPM_TOKEN}` is a literal value and should not be replaced. It will be replaced by `npm` at runtime with the value of that environment variable

## Client handoff

This is a bit tricky. We have some options:

- We may be able to open-source dependencies
- We may be able to use some license trickery
- We may be able to use client-specific versioning, i.e. creating a `0.0.0-${CLIENT_NAME}.0` version on handoff
- We may be able to simply hand off source code via a GitHub organization, at which point we'd find & replace any dependency scopes with the new organization's scope
