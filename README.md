# vue-router-4-query-types

A reproduction of well-typed code that fails due to an incorrect type in Vue Router 4.

## Steps to reproduce

1. `npm install`
2. `npm run typecheck`
3. `npm run dev`
4. Go to `/` and check for errors.
5. Go to `/?foo=bar` and check for errors.

## What is expected

Step 3 should show that the code does not type-check, as it includes dangerous code that should produce an error in step 4.

## What is actually happening

Step 3 shows that the code type-checks. The bug, reproduced in step 4, comes from `route.query.foo` being `undefined`, despite `route.query.foo` not being inhabited by `undefined`.

## Fix

The type of `LocationQuery` should be changed from:

```ts
export type LocationQueryValue = string | null

export type LocationQuery = Record<
  string,
  LocationQueryValue | LocationQueryValue[]
>
```

to

```ts
export type LocationQueryValue = string | null

export type LocationQuery = Record<
  string,
  LocationQueryValue | LocationQueryValue[] | undefined
>
```

`LocationQueryRaw` is defined as follows, meaning that it probably does not need to change:

```ts
export type LocationQueryValueRaw = LocationQueryValue | number | undefined

export type LocationQueryRaw = Record<
  string | number,
  LocationQueryValueRaw | LocationQueryValueRaw[]
>
```
