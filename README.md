# Loaders Team

## Purpose

The Node.js Loaders Team maintains and actively develops the ECMAScript Modules Loaders implementation in Node.js core.

## History

This team is spun off from the [Modules team](https://github.com/nodejs/modules). We aim to implement the [use cases](https://github.com/nodejs/modules/blob/main/doc/use-cases.md) that went unfulfilled by the initial ES modules implementation that can be achieved via loaders.

## Project

- [Resources](doc/resources.md)

- [Use cases](./doc/use-cases.md)

- [Design](./doc/design/overview.md)

- [Project board](https://github.com/nodejs/node/projects/17)

- [Meeting minutes](./doc/meetings)

## Status

- [x] Finish https://github.com/nodejs/node/pull/37468 / https://github.com/nodejs/node/pull/35524, simplifying the hooks to `resolve`, `load` and `globalPreloadCode`.

- [x] Refactor the internal Node ESMLoader hooks into `resolve` and `load`. Node’s internal loader already has no-ops for `transformSource` and `getGlobalPreloadCode`, so all this really entails is wrapping the internal `getFormat` and `getSource` with one function `load` (`getFormat` is used internally outside ESMLoader, so they cannot merely be merged). https://github.com/nodejs/node/pull/37468

- [x] Refactor Node’s internal ESM loader to move its exception on unknown file types from within `resolve` (on detection of unknown extensions) to within `load` (if the resolved extension has no defined translator). https://github.com/nodejs/node/pull/37468

- [x] Implement chaining as described in the [design](doc/design/proposal-chaining-middleware.md), where the `default<hookName>` becomes `next` and references the next registered hook in the chain. https://github.com/nodejs/node/pull/42623

- [ ] Convert `resolve` from async to sync

   - [ ] Add an async `resolve` to [`module`](https://nodejs.org/api/module.html) module

- [ ] Add helper/utility functions (eg `getPackageType()`) to [`module`](https://nodejs.org/api/module.html) module

- [ ] Move loaders off thread

- [ ] Get a `load` return value of `format: 'commonjs'` to work, or at least error informatively. See https://github.com/nodejs/node/issues/34753#issuecomment-735921348.

After this, we should get user feedback regarding the developer experience; for example, is too much boilerplate required? Should we have a separate `transform` hook? And so on. We should also investigate and potentially implement the [technical improvements](doc/use-cases.md#improvements) on our to-do list, such as the loaders-application communication channel and moving loaders into a thread.
