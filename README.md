# `@rollup/plugin-typescript` missing declaration file reproduction

Repository to reproduce [this issue](https://github.com/rollup/plugins/issues/1554) in `@rollup/plugin-typescript`.

## Context

When the [`include` compiler option](https://www.typescriptlang.org/tsconfig#include) is specified in `tsconfig.json`, `@rollup/plugin-typescript` does not emit declaration files (`.d.ts`) for source files which:

1. Only contain types (i.e., does not contain any code which is transpiled to JavaScript); and
1. Are not part of the filenames or patterns in the `include` compiler option.

For source files which meet the above conditions, the type definitions in these files are not part of the Rollup build output even if they are imported/exported from another source file that is part of the filenames or patterns in the `include` compiler option.

This issue does not occur with the TypeScript compiler because files that meet the above conditions but are imported/exported from another source file that is part of the filenames or patterns in the `include` compiler option are still emitted.

Given the same `tsconfig.json`, I would expect the same set of type definitions to be outputted by `@rollup/plugin-typescript` & the TypeScript compiler.

## Setting up

1. Run `npm i`.

## Expected behaviour (Rollup)

1. Run `npm run build-tsc`.
1. Observe that `type-only.d.ts` is generated in `dist`.

## Actual behaviour (TypeScript compiler)

1. Run `npm run build-rollup`.
1. Observe that `type-only.d.ts` is not generated in `dist`.
   In `index.d.ts`, the `MyNumber` type import is missing.
