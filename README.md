# `pre`
A simple dev code preprocessor.

## What it does

`pre` is designed to copy a codebase and strip out development code from JavaScript (and other languages which support "C-style" multiline comments). 

The preprocessor enacts the following rules:

- Any lines which contain the string `/*PRE*/` will be removed.
- Multiline preprocessor blocks are any lines between (and including) `/*PRE-START*/` and `/*PRE-END*/`. `/*PRE-START*/` and `/*PRE-END*/` must be on separate lines.

## How to install

`$ gem install pre-0.1.gem`

## How to use

`$ pre {input directory} {output directory}`

## Why?

I built this to inline unit tests on small Javascript projects I work on. I would like to include quick and dirty tests near function definitions, but then be able to strip them out later. With `pre`, I can include these tests as development code marked with `/*PRE*/` (or between and including lines containing `/*PRE-START*/` and `/*PRE-END*/`) right up next to the function/object definition, reminding me to update my assertions as I update my code. The following example illustrates the intended use case.

## Example

The file `input/test.js`
```javascript
/*PRE-START*/ // This line and the following lines will be removed
function assert(trueExpression, failureMessage) {
	if(!trueExpression) {
		throw failureMessage;
	}
}
/*PRE-END*/ // This line will be removed
// This line will not be removed

// This is the function we will test with assert
function square(x) {
	return x * x;
}
// The following line will be removed
assert(square(5) === 25, "5 x 5 should be 25"); /*PRE*/
```

Processed with `$ pre input output` will produce:

```javascript
// This line will not be removed

// This is the function we will test with assert
function square(x) {
	return x * x;
}
// The following line will be removed
```