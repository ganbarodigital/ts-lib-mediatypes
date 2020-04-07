# MediaTypes Library for Typescript

## Introduction

This TypeScript library provides a `MediaType` type that supports [RFC 2045][RFC 2045] and [RFC 6838][RFC 6838] MediaType strings:

- [Introduction](#introduction)
- [Quick Start](#quick-start)
- [V1 API](#v1-api)
  - [MediaType Value Type](#mediatype-value-type)
  - [MediaTypeParts](#mediatypeparts)
  - [isMediaType()](#ismediatype)
  - [parseMediaType()](#parsemediatype)
  - [mustBeMediaType()](#mustbemediatype)
- [NPM Scripts](#npm-scripts)
  - [npm run clean](#npm-run-clean)
  - [npm run build](#npm-run-build)
  - [npm run test](#npm-run-test)
  - [npm run cover](#npm-run-cover)

## Quick Start

```
# run this from your Terminal
npm install @ganbarodigital/ts-lib-mediatypes
```

```typescript
// add this import to your Typescript code
import { MediaType } from "@ganbarodigital/ts-lib-mediatypes/lib/v1"
```

__VS Code users:__ once you've added a single import anywhere in your project, you'll then be able to auto-import anything else that this library exports.

## V1 API

### MediaType Value Type

```typescript
// how to import this into your own code
import { MediaType } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

// base class
import { RefinedString } from "@ganbarodigital/ts-lib-value-objects/lib/v2";

// types used for parameters
import { OnError, THROW_THE_ERROR } from "@ganbarodigital/ts-lib-error-reporting/lib/v1";

export class MediaType extends RefinedString {
    /**
     * smart constructor.
     *
     * calls your `onError` handler if `input` isn't a well-formatted
     * media type
     */
    public constructor(input: string, onError: OnError = THROW_THE_ERROR);

    /**
     * returns a breakdown of the individual components for this media type
     */
    public parse(onError: OnError = THROW_THE_ERROR): MediaTypeParts;
}
```

`MediaType` is a _value type_. It represents an RFC-compliant media type string that has been successfully validated.

Here's how to create it, and how to use it:

```typescript
// how to import this into your own code
import { MediaType } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

// create a value object
const myMediaType = new MediaType("text/html; charset=UTF-8");

// it will auto-convert to a string in most places
console.log("myMediaType is: " + myMediaType);

// if you run into any auto-convert problems, call `.valueOf()`:
const parts = parseMediaType(myMediaType.valueOf());
```

### MediaTypeParts Value Type

```typescript
// how to import this into your own code
import { MediaTypeParts } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

export interface MediaTypeParts {
    type: string;
    tree?: string;
    subtype: string;
    suffix?: string;
    parameters?: {[parameter: string]: string};
}
```

`MediaTypeParts` is a _value type_. It contains the parsed contents of a MediaType.

Call [`parseMediaType()`](#parsemediatype) to get a `MediaTypeParts` object from your MediaType.

### isMediaType()

```typescript
// how to import this into your own code
import { isMediaType } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

/**
 * Data guard. Returns `true` if the input string matches the structure
 * of an RFC2046 / RFC6838 media type.
 *
 * @param input
 */
export function isMediaType(input: string): boolean;
```

`isMediaType()` is a _data guard_. Use it to prove that a string contains something with the structure of a MediaType:

    type "/" [tree "."] subtype ["+" suffix] *[";" parameter]

### parseMediaType()

```typescript
// how to import this into your own code
import { parseMediaType } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

// types used for parameters
import { OnError, THROW_THE_ERROR } from "@ganbarodigital/ts-lib-error-reporting/lib/v1";

/**
 * Data parser. Breaks down an RFC-compliant MediaType into its
 * individual parts.
 */
export function parseMediaType(
    input: string,
    onError: OnError = THROW_THE_ERROR,
): MediaTypeParts;
```

`parseMediaType()` is a _data parser_. Use it to break down the contents of a media type into its individual parts.

### mustBeMediaType()

```typescript
// how to import this into your own code
import { mustBeMediaType } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

// types used for parameters
import { OnError, THROW_THE_ERROR } from "@ganbarodigital/ts-lib-error-reporting/lib/v1";

/**
 * Data guarantee. Calls your onError handler if the given input
 * isn't an RFC-2045 / 6838-compliant MediaType
 *
 * @param input
 *        this string to be validated
 */
export function mustBeMediaType(input: string, onError: OnError = THROW_THE_ERROR): void;
```

`mustBeMediaType()` is a _data guarantee_. Use it to ensure that the given string has the structure of a RFC-compliant media type.

### MediaTypeMatchRegexIsBroken Error

```typescript
// how to import this into your own code
import { MediaTypeMatchRegexIsBrokenError } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

// base class
import { AppError, AppErrorParams } from "@ganbarodigital/ts-lib-error-reporting/lib/v1";

export class MediaTypeMatchRegexIsBrokenError extends AppError {
    public constructor(params: AppErrorParams);
}
```

`MediaTypeMatchRegexIsBroken` is a throwable, structured `Error`. It's thrown whenever we break one of the regexes that we use to parse media type strings.

This is an internal error.

### NotAMediaType Error

```typescript
// how to import this into your own code
import { NotAMediaTypeError } from "@ganbarodigital/ts-lib-mediatypes/lib/v1";

// base class
import { AppError, AppErrorParams } from "@ganbarodigital/ts-lib-error-reporting/lib/v1";

// params object structure
export interface NotAMediaTypeExtraData {
    public: {
        input: string;
    };
}

export class NotAMediaTypeError extends AppError {
    public constructor(params: NotAMediaTypeExtraData & AppErrorParams);
}
```

`NotAMediaType` is a throwable, structured `Error`. It's thrown whenever a string doesn't have the expected structure of a media type.

## NPM Scripts

### npm run clean

Use `npm run clean` to delete all of the compiled code.

### npm run build

Use `npm run build` to compile the Typescript into plain Javascript. The compiled code is placed into the `lib/` folder.

`npm run build` does not compile the unit test code.

### npm run test

Use `npm run test` to compile and run the unit tests. The compiled code is placed into the `lib/` folder.

### npm run cover

Use `npm run cover` to compile the unit tests, run them, and see code coverage metrics.

Metrics are written to the terminal, and are also published as HTML into the `coverage/` folder.

[RFC 2045]: https://tools.ietf.org/html/rfc2045
[RFC 6838]: https://tools.ietf.org/html/rfc6838