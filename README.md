# Repro case for broken `yarn add` when using workspaces

## Installing

Clone this repo and run `yarn install`.

## Description

Minimal monorepo with two packages: `sdeleon28-a` and `sdeleon28-b`. `sdeleon28-b` depends on `sdeleon28-a`. When attempting to add an external npm package as a second dependency of `sdeleon28-b`, yarn errors out.

## Reproducing the bug

Once installed, `cd` into `packages/sdeleon28-b` and run `yarn add right-pad`.

**Expected behavior**

Yarn should:

* Install `right-pad` to the top-level `node_modules/` directory.
* Update `packages/sdeleon28-b/package.json`.
* Update top-level `yarn.lock`.

**Actual behavior**

```
$ yarn add right-pad
yarn add v0.28.4
[1/4] 🔍  Resolving packages...
error Couldn't find package "sdeleon28-a" on the "npm" registry.
info Visit https://yarnpkg.com/en/docs/cli/add for documentation about this command.
```

It looks like yarn cannot tell `sdeleon28-a` is a private package and queries the npm registry instead.

