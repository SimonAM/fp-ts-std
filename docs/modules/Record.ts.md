---
title: Record.ts
nav_order: 24
parent: Modules
---

## Record overview

Various functions to aid in working with `Record`s and more broadly objects.

Added in v0.1.0

---

<h2 class="text-delta">Table of contents</h2>

- [utils](#utils)
  - [invertAll](#invertall)
  - [invertLast](#invertlast)
  - [lookupV](#lookupv)
  - [merge](#merge)
  - [omit](#omit)
  - [pick](#pick)
  - [pickFrom](#pickfrom)
  - [reject](#reject)
  - [values](#values)

---

# utils

## invertAll

Invert a record, collecting values with duplicate keys in an array. Should
you only care about the last item or are not worried about the risk of
duplicate keys, see instead `invertLast`.

**Signature**

```ts
export declare const invertAll: <A>(f: (x: A) => string) => (x: Record<string, A>) => Record<string, Array<string>>
```

```hs
invertAll :: (a -> string) -> Record string a -> Record string (Array string)
```

**Example**

```ts
import { invertAll } from 'fp-ts-std/Record'
import { fromNumber } from 'fp-ts-std/String'

assert.deepStrictEqual(invertAll(fromNumber)({ a: 1, b: 2, c: 2, d: 3 }), { '1': ['a'], '2': ['b', 'c'], '3': ['d'] })
```

Added in v0.7.0

## invertLast

Invert a record, keeping only the last value should the same key be
encountered more than once. If you'd like to keep the values that would be
lost, see instead `invertAll`.

**Signature**

```ts
export declare const invertLast: <A>(f: (x: A) => string) => (x: Record<string, A>) => Record<string, string>
```

```hs
invertLast :: (a -> string) -> Record string a -> Record string string
```

**Example**

```ts
import { invertLast } from 'fp-ts-std/Record'
import { fromNumber } from 'fp-ts-std/String'

assert.deepStrictEqual(invertLast(fromNumber)({ a: 1, b: 2, c: 2, d: 3 }), { '1': 'a', '2': 'c', '3': 'd' })
```

Added in v0.7.0

## lookupV

Like `fp-ts/Record::lookup` but flipped, which the "V" suffix denotes.

**Signature**

```ts
export declare const lookupV: <A>(x: Record<string, A>) => (k: string) => Option<A>
```

```hs
lookupV :: Record string a -> string -> Option a
```

**Example**

```ts
import { lookupV } from 'fp-ts-std/Record'
import * as A from 'fp-ts/Array'

const x = { a: 1, b: 'two', c: [true] }
const ks = ['a', 'c']

assert.deepStrictEqual(A.filterMap(lookupV(x))(ks), [1, [true]])
```

Added in v0.1.0

## merge

Merge two records together. For merging many identical records, instead
consider defining a semigroup.

**Signature**

```ts
export declare const merge: <A>(x: A) => <B>(y: B) => A & B
```

```hs
merge :: a -> b -> a & b
```

**Example**

```ts
import { merge } from 'fp-ts-std/Record'

assert.deepStrictEqual(merge({ a: 1, b: 2 })({ b: 'two', c: true }), { a: 1, b: 'two', c: true })
```

Added in v0.7.0

## omit

Omit a set of keys from a `Record`. The value-level equivalent of the `Omit`
type.

**Signature**

```ts
export declare const omit: <K extends string>(
  ks: K[]
) => <V, A extends Record<K, V>>(x: Partial<A>) => Pick<A, Exclude<keyof A, K>>
```

```hs
omit :: k extends string, a extends (Record k v) => Array k -> Partial a -> Pick a (Exclude (keyof a) k)
```

**Example**

```ts
import { omit } from 'fp-ts-std/Record'

const sansB = omit(['b'])

assert.deepStrictEqual(sansB({ a: 1, b: 'two', c: [true] }), { a: 1, c: [true] })
```

Added in v0.1.0

## pick

Pick a set of keys from a `Record`. The value-level equivalent of the `Pick`
type. Returns undefined on missing values.

**Signature**

```ts
export declare const pick: <A, K extends keyof A>(ks: K[]) => (x: A) => Pick<A, K>
```

```hs
pick :: k extends (keyof a) => Array k -> a -> Pick a k
```

**Example**

```ts
import { pick } from 'fp-ts-std/Record'
import { pipe } from 'fp-ts/function'

const picked = pipe({ a: 1, b: 'two', c: [true] }, pick(['a', 'c']))
const pickedMissing = pipe({ a: 1, b: 'two' }, pick(['a', 'c']))
assert.deepStrictEqual(picked, { a: 1, c: [true] })
assert.deepStrictEqual(picked, { a: 1, c: undefined })

```

Added in v0.1.0

## pickFrom

Like `pick`, but allows you to specify the input record upfront.

**Signature**

```ts
export declare const pickFrom: <A>() => <K extends keyof A>(ks: K[]) => (x: A) => Pick<A, K>
```

```hs
pickFrom :: k extends (keyof a) => () -> Array k -> a -> Pick a k
```

**Example**

```ts
import { pickFrom } from 'fp-ts-std/Record'

type MyType = { a: number; b: string; c: ReadonlyArray<boolean> }
const picked = pickFrom<MyType>()(['a', 'c'])

assert.deepStrictEqual(picked({ a: 1, b: 'two', c: [true] }), { a: 1, c: [true] })
```

Added in v0.12.0

## reject

Filters out key/value pairs in the record for which the predicate upon the
value holds. This can be thought of as the inverse of ordinary record
filtering.

**Signature**

```ts
export declare const reject: <A>(f: Predicate<A>) => Endomorphism<Record<string, A>>
```

```hs
reject :: Predicate a -> Endomorphism (Record string a)
```

**Example**

```ts
import { reject } from 'fp-ts-std/Record'
import { Predicate } from 'fp-ts/Predicate'

const isEven: Predicate<number> = (n) => n % 2 === 0

assert.deepStrictEqual(reject(isEven)({ a: 1, b: 2, c: 3, d: 4 }), { a: 1, c: 3 })
```

Added in v0.7.0

## values

Get the values from a `Record`.

**Signature**

```ts
export declare const values: <A>(x: Record<string, A>) => A[]
```

```hs
values :: Record string a -> Array a
```

**Example**

```ts
import { values } from 'fp-ts-std/Record'

const x = { a: 1, b: 'two' }

assert.deepStrictEqual(values(x), [1, 'two'])
```

Added in v0.1.0
