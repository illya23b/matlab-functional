# matlab-functional

Functional programming tools for MATLAB.

This is a collection of utilities to promote a more functional style in
MATLAB. It is similar in intent to
[smschm’s functional-matlab code](https://github.com/smschm/functional-matlab),
but it’s a little gentler, and it’s more focused on practical tasks than on
building blocks. It provides useful MATLAB versions of such functional stand-bys
as `map`, `fold`, `ifthen`, and `curry` (that one was *not* easy!), and a very
useful function called `bindin`, which gets around some MATLAB weirdnesses.

## Mapping

Sure, MATLAB has `arrayfun` and `cellfun`, but those are cumbersome to use. You
know you’ll only use mapping if it’s easy to do. And it should be easy to ask
for a cell array as output — you shouldn’t have to type out `'UniformOutput',
false` each time.

To make this easier, I have two simple functions, `map` and `mapc`. They’re
called just like `arrayfun` or `cellfun`; they automatically call `cellfun` if
passed a cell array, and `arrayfun` otherwise. `map` returns an array; `mapc`
returns a cell array. It’s that simple.

There are also two “safe” versions of these functions, `maps` and `mapcs`. If
your function raises an exception for any of the inputs, the corresponding
outputs are set to `nan`.

To summarize:
 - `map` takes an array or cell array, and returns an array.
 - `mapc` takes an array or cell array, and returns a cell array.
 - `maps` is like `map`, but replaces exceptions with `nan`.
 - `mapcs` is like `mapc`, but replaces exceptions with `{nan}`.

A few examples:
```matlab
 arrays = {[1:5], [4:7], [2:9]};
 squares_of_first = map(@(x) x**2, arrays{1});
 squares_of_each = mapc(@(array) map(@(x) x**2, array), arrays);
 means = map(mean, arrays);
```


## Reducing / folding

What comes after mapping? Reducing! Or "fold," as it's called in some
languages. Fold works by starting with an initial value, then applying a binary
operator to combine successive values. So, for instance, these are equivalent:
```matlab
 array = 1:4;
 S = sum(array);
 S = fold(@(x, y) x + y, array);
 S = (((1 + 2) + 3) + 4);
```
Of course, `fold` can take any function as its first argument. It can be called
either with an array or with a cell array as its second argument.
