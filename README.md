# @sa-community/addons-data

This repository contains [the script](.github/workflows/update.yml) that generates [@sa-community/addons-data](https://npmjs.com/package/@sa-community/addons-data).

## Usage

The package exports an array of objects `{ addonId: string, manifest: AddonManifest }[]`.
For example, to get all addon manifests with the `forums` tag:

```js
const addons = require("@sa-community/addons-data");
addons.filter((addon) => addon.manifest.tags.includes("forums")).map((addon) => addon.manifest);
```

Addon IDs are not in the manifest, so if you wanted to get all of their IDs:

```js
const addons = require("@sa-community/addons-data");
addons.filter((addon) => addon.manifest.tags.includes("forums")).map((addon) => addon.addonId);
```

### Types

The package, including addon manifests, is fully typed.
The [addon manifest typedef](types.d.ts) is based off [manifest-schema](https://github.com/ScratchAddons/manifest-schema), and I try to keep them as similar as possible.

All [addon manifests on Scratch Addons's `master` branch](https://github.com/ScratchAddons/ScratchAddons/tree/master/addons) are typechecked against that typedef every 12 hours.
If there are any errors, an issue is automatically created.
Issues indicate either that the typedef needs to be updated or a mistake in an addon manifest.

#### Typedef vs JSON Schema

Everything the typedef checks is also checked by the JSON Schema, but there are a few small things the typedef can't check easily:

- Patterns for colors, links, and IDs
- Minimums for numerical values
- Minimum lengths for string values
- Minimum item counts for array values
- Minimum property counts for record values
- No extraneous properties
- Every addon having one of the "main" tags (community, editor, player, popup)

The main advantage of using the typedef over the schema for manifest verification is ease of maintainability.
TypeScript typedefs are also more portable and can be used in more contexts than a JSON schema could be.

### Importing a single addon's data

You can also import just one addon by its ID:

```js
const addon = require("@sa-community/addons-data/addons/semicolon");
console.log(addon.name); // Semicolon glitch
console.log(addon.credits[0].name); // GrahamSH
```

Note that while the package is typed, each addon is typed generically to avoid breaking changes.
Meaning, the type of each addon is the generic `AddonManifest`, and the types do not define specific keys for specific addons.

### Importing the manifest

This package also exports the extension manifest:

```js
const addon = require("@sa-community/addons-data/manifest.json");
console.log(manifest.version); // 1.42.0
console.log(manifest.homepage_url); // https://scratchaddons.com
```
