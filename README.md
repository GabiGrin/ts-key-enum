# ts-key-enum

<a href="https://gitlab.com/nfriend/ts-key-enum/pipelines/latest" target="_blank">
    <img src="https://gitlab.com/nfriend/ts-key-enum/badges/master/pipeline.svg" alt="GitLab build status">
</a>

A TypeScript string enum for compile-time safety when working with `event.key`.

<img align="right" src="https://raw.githubusercontent.com/nfriend/ts-key-enum/master/logo.jpg" />

## Install

```sh
npm install ts-key-enum --save
```

## Purpose

Tired of referencing keyboard keys with a string?

```js
onKeyPress = (ev) => {

    // whoops, it's actually ArrowLeft!
    if (ev.key === 'LeftArrow') {
        ...
    }
}
```

Me too. With this module, you can do this instead (in a TypeScript file):

```ts
onKeyPress = (ev) => {

    // much better
    if (ev.key === Key.ArrowLeft) {
        ...
    }
}
```

## Background

This is similar to the [ts-keycode-enum](https://github.com/nfriend/ts-keycode-enum) module, but it provides an enum with string values that correspond with the [`event.key` values](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) instead of number values that correspond to the deprecated [`event.which`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/which) and [`event.keyCode`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode) values.

## Usage

To use this module, import the `Key` enum at the top of your TypeScript file:

```js
import { Key } from 'ts-key-enum';
```

You can now use the enum value in place of key strings throughout the file:

```js
// if (ev.key === 'Escape') { ... }
if (ev.key === Key.Escape) { ... }
```

See [`Key.enum.d.ts`](./Key.enum.d.ts) for a complete list of available keys. This file is auto-generated from the list of keys found at MDN: https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values.

## What's included

The `Key` enum contains values for all standard non-printable keys such as "CapsLock", "Backspace", and "AudioVolumeMute". The enum does _not_ contain values for printable keys such as "a", "A", "#", "é", or "¿", simply because the list of possible values is too vast to include in a single enum. To test for printable values, simply use a string comparison:

```js
if (ev.key === 'é') { ... }
```

## v2 vs v3

This package is published as two versions on [NPM](https://www.npmjs.com/package/ts-key-enum): `v2.x` and `v3.x`.

### v2

`v2.x` defines the `Key` enum as [a "basic" enum](https://www.typescriptlang.org/docs/handbook/enums.html#enums):

```ts
export enum Key { ... }
```

The end result is a JavaScript object that contains every enum value. You can see this object here: https://gitlab.com/nfriend/ts-key-enum/blob/v2/dist/js/Key.enum.js.

### v3

`v3.x` defines the `Key` enum as [a `const` enum](https://www.typescriptlang.org/docs/handbook/enums.html#const-enums):

```ts
export const enum Key { ... }
```

This allows the enum's definition to live entirely in the definition file `https://gitlab.com/nfriend/ts-key-enum/blob/master/Key.enum.d.ts`. Consumers can use this enum without including _all_ of the enum's values in their own JavaScript bundle; only the values referenced in their code will be injected into their output file.

## Which version should I use?

It's advisable to use `v3.x` unless you have a reason to use `v2.x`. Some reasons you may need to use `v2.x`:

- You are using this package as a JavaScript module (not a TypeScript module).
- You are using TypeScript < `1.4`, which doesn't support `const enum`.

## Building

To build this module yourself, first install its dependencies using:

```sh
npm install
```

Next, run the scraper script ([`scrapeMDNForKeys.ts`](./scrapeMDNForKeys.ts)) using:

```sh
npm run scrape
```

This will overwrite [`Key.enum.d.ts`](./Key.enum.d.ts) with the updated list of keys found in MDN.

Verify that the enum builds without any TypeScript errors:

```sh
npm run build
```

Finally, make sure the enum passes all linters:

```sh
npm run lint
```

## License

MIT
