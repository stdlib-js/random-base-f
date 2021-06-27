<!--

@license Apache-2.0

Copyright (c) 2018 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->

# F Random Numbers

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] [![dependencies][dependencies-image]][dependencies-url]

> [F][f] distributed pseudorandom numbers.

<section class="installation">

## Installation

```bash
npm install @stdlib/random-base-f
```

</section>

<section class="usage">

## Usage

```javascript
var f = require( '@stdlib/random-base-f' );
```

#### f( d1, d2 )

Returns a pseudorandom number drawn from an [F][f] distribution with parameters `d1` (degrees of freedom) and `d2` (degrees of freedom).

```javascript
var r = f( 2.0, 5.0 );
// returns <number>
```

If `d1 <= 0` or `d2 <= 0`, the function returns `NaN`.

```javascript
var r = f( 2.0, -2.0 );
// returns NaN

r = f( -2.0, 2.0 );
// returns NaN
```

If `d1` or `d2` is `NaN`, the function returns `NaN`.

```javascript
var r = f( NaN, 5.0 );
// returns NaN

r = f( 2.0, NaN );
// returns NaN
```

#### f.factory( \[d1, d2, ]\[options] )

Returns a pseudorandom number generator (PRNG) for generating pseudorandom numbers drawn from am [F][f] distribution.

```javascript
var rand = f.factory();

var r = rand( 1.5, 1.5 );
// returns <number>
```

If provided `d1` and `d2`, the returned generator returns random variates from the specified distribution.

```javascript
// Draw from F( 1.5, 1.5 ) distribution:
var rand = f.factory( 1.5, 1.5 );

var r = rand();
// returns <number>

r = rand();
// returns <number>
```

If not provided `d1` and `d2`, the returned generator requires that both parameters be provided at each invocation.

```javascript
var rand = f.factory();

var r = rand( 1.0, 1.0 );
// returns <number>

r = rand( 3.14, 2.25 );
// returns <number>
```

The function accepts the following `options`:

-   **prng**: pseudorandom number generator for generating uniformly distributed pseudorandom numbers on the interval `[0,1)`. If provided, the function **ignores** both the `state` and `seed` options. In order to seed the returned pseudorandom number generator, one must seed the provided `prng` (assuming the provided `prng` is seedable).
-   **seed**: pseudorandom number generator seed.
-   **state**: a [`Uint32Array`][@stdlib/array/uint32] containing pseudorandom number generator state. If provided, the function ignores the `seed` option.
-   **copy**: `boolean` indicating whether to copy a provided pseudorandom number generator state. Setting this option to `false` allows sharing state between two or more pseudorandom number generators. Setting this option to `true` ensures that a returned generator has exclusive control over its internal state. Default: `true`.

To use a custom PRNG as the underlying source of uniformly distributed pseudorandom numbers, set the `prng` option.

```javascript
var minstd = require( '@stdlib/random-base-minstd' );

var rand = f.factory({
    'prng': minstd.normalized
});

var r = rand( 2.0, 3.0 );
// returns <number>
```

To seed a pseudorandom number generator, set the `seed` option.

```javascript
var rand1 = f.factory({
    'seed': 12345
});

var r1 = rand1( 2.0, 3.0 );
// returns <number>

var rand2 = f.factory( 2.0, 3.0, {
    'seed': 12345
});

var r2 = rand2();
// returns <number>

var bool = ( r1 === r2 );
// returns true
```

To return a generator having a specific initial state, set the generator `state` option.

```javascript
var rand;
var bool;
var r;
var i;

// Generate pseudorandom numbers, thus progressing the generator state:
for ( i = 0; i < 1000; i++ ) {
    r = f( 2.0, 3.0 );
}

// Create a new PRNG initialized to the current state of `f`:
rand = f.factory({
    'state': f.state
});

// Test that the generated pseudorandom numbers are the same:
bool = ( rand( 2.0, 3.0 ) === f( 2.0, 3.0 ) );
// returns true
```

#### f.NAME

The generator name.

```javascript
var str = f.NAME;
// returns 'f'
```

#### f.PRNG

The underlying pseudorandom number generator.

```javascript
var prng = f.PRNG;
// returns <Function>
```

#### f.seed

The value used to seed `f()`.

```javascript
var rand;
var r;
var i;

// Generate pseudorandom values...
for ( i = 0; i < 100; i++ ) {
    r = f( 2.0, 2.0 );
}

// Generate the same pseudorandom values...
rand = f.factory( 2.0, 2.0, {
    'seed': f.seed
});
for ( i = 0; i < 100; i++ ) {
    r = rand();
}
```

If provided a PRNG for uniformly distributed numbers, this value is `null`.

<!-- eslint-disable stdlib/no-builtin-math -->

```javascript
var rand = f.factory({
    'prng': Math.random
});

var seed = rand.seed;
// returns null
```

#### f.seedLength

Length of generator seed.

```javascript
var len = f.seedLength;
// returns <number>
```

If provided a PRNG for uniformly distributed numbers, this value is `null`.

<!-- eslint-disable stdlib/no-builtin-math -->

```javascript
var rand = f.factory({
    'prng': Math.random
});

var len = rand.seedLength;
// returns null
```

#### f.state

Writable property for getting and setting the generator state.

```javascript
var r = f( 2.0, 3.0 );
// returns <number>

r = f( 2.0, 3.0 );
// returns <number>

// ...

// Get a copy of the current state:
var state = f.state;
// returns <Uint32Array>

r = f( 2.0, 3.0 );
// returns <number>

r = f( 2.0, 3.0 );
// returns <number>

// Reset the state:
f.state = state;

// Replay the last two pseudorandom numbers:
r = f( 2.0, 3.0 );
// returns <number>

r = f( 2.0, 3.0 );
// returns <number>

// ...
```

If provided a PRNG for uniformly distributed numbers, this value is `null`.

<!-- eslint-disable stdlib/no-builtin-math -->

```javascript
var rand = f.factory({
    'prng': Math.random
});

var state = rand.state;
// returns null
```

#### f.stateLength

Length of generator state.

```javascript
var len = f.stateLength;
// returns <number>
```

If provided a PRNG for uniformly distributed numbers, this value is `null`.

<!-- eslint-disable stdlib/no-builtin-math -->

```javascript
var rand = f.factory({
    'prng': Math.random
});

var len = rand.stateLength;
// returns null
```

#### f.byteLength

Size (in bytes) of generator state.

```javascript
var sz = f.byteLength;
// returns <number>
```

If provided a PRNG for uniformly distributed numbers, this value is `null`.

<!-- eslint-disable stdlib/no-builtin-math -->

```javascript
var rand = f.factory({
    'prng': Math.random
});

var sz = rand.byteLength;
// returns null
```

#### f.toJSON()

Serializes the pseudorandom number generator as a JSON object.

```javascript
var o = f.toJSON();
// returns { 'type': 'PRNG', 'name': '...', 'state': {...}, 'params': [] }
```

If provided a PRNG for uniformly distributed numbers, this method returns `null`.

<!-- eslint-disable stdlib/no-builtin-math -->

```javascript
var rand = f.factory({
    'prng': Math.random
});

var o = rand.toJSON();
// returns null
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   If PRNG state is "shared" (meaning a state array was provided during PRNG creation and **not** copied) and one sets the generator state to a state array having a different length, the PRNG does **not** update the existing shared state and, instead, points to the newly provided state array. In order to synchronize PRNG output according to the new shared state array, the state array for **each** relevant PRNG must be **explicitly** set.
-   If PRNG state is "shared" and one sets the generator state to a state array of the same length, the PRNG state is updated (along with the state of all other PRNGs sharing the PRNG's state array).

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var f = require( '@stdlib/random-base-f' );

var seed;
var rand;
var i;

// Generate pseudorandom numbers...
for ( i = 0; i < 100; i++ ) {
    console.log( f( 2.0, 2.0 ) );
}

// Create a new pseudorandom number generator...
seed = 1234;
rand = f.factory( 6.0, 2.0, {
    'seed': seed
});
for ( i = 0; i < 100; i++ ) {
    console.log( rand() );
}

// Create another pseudorandom number generator using a previous seed...
rand = f.factory( 2.0, 2.0, {
    'seed': f.seed
});
for ( i = 0; i < 100; i++ ) {
    console.log( rand() );
}
```

</section>

<!-- /.examples -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2021. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/random-base-f.svg
[npm-url]: https://npmjs.org/package/@stdlib/random-base-f

[test-image]: https://github.com/stdlib-js/random-base-f/actions/workflows/test.yml/badge.svg
[test-url]: https://github.com/stdlib-js/random-base-f/actions/workflows/test.yml

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/random-base-f/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/random-base-f?branch=main

[dependencies-image]: https://img.shields.io/david/stdlib-js/random-base-f.svg
[dependencies-url]: https://david-dm.org/stdlib-js/random-base-f/main

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://gitter.im/stdlib-js/stdlib/

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/random-base-f/main/LICENSE

[f]: https://en.wikipedia.org/wiki/F_distribution

[@stdlib/array/uint32]: https://github.com/stdlib-js/array-uint32

</section>

<!-- /.links -->
