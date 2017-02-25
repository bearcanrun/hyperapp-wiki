[Hyperx](https://github.com/substack/hyperx) is a standards-compliant ES6 tagged [template string function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals) factory. It is a pure JavaScript alternative to JSX.

Hyperx looks like this:

```jsx
const hello = html`
    <div>
        <h1>Hello.</h1>
        <button
            onclick=${action}
        >Click</button>
    </div>`
```

To generate the `html` function, you can use the following boilerplate:

```jsx
const { h, app } = require("hyperapp")
const hyperx = require("hyperx")
const html = hyperx(h)
```

## Setup

We'll use [Hyperxify](https://github.com/substack/hyperxify) to transform Hyperx into native HyperApp [h](/hyperapp/hyperapp/wiki/api#h) function calls and a bundler to create a single `bundle.js` file we can deliver to the browser.

There are caveats, however, the ES6 module syntax is incompatible with Hyperxify, so you have to use Node.js `require` function.

In a new directory, create an `index.html` file:

```html
<!doctype html>
<html lang="en">
<head>
    <title>Hello HyperApp</title>
</head>
<body>
    <script src="bundle.js"></script>
</body>
</html>
```

And an `index.js` file:

```jsx
const { h, app } = require("hyperapp")
const hyperx = require("hyperx")
const html = hyperx(h)

app({
    model: "Hi.",
    view: model => html`<h1>${model}</h1>`
})
```

Install dependencies:
<pre>
npm i -S <a href="https://www.npmjs.com/package/hyperapp">hyperapp</a>
</pre>

### Browserify

Install development dependencies:
<pre>
npm i -D \
    <a href="https://www.npmjs.com/package/browserify">browserify</a> \
    <a href="https://www.npmjs.com/package/hyperx">hyperx</a> \
    <a href="https://www.npmjs.com/package/hyperxify">hyperxify</a> \
    <a href="https://www.npmjs.com/package/babelify">babelify</a> \
    <a href="https://www.npmjs.com/package/uglifyify">uglifyify</a> \
    <a href="https://www.npmjs.com/package/bundle-collapser">bundle-collapser</a>
    <a href="https://www.npmjs.com/package/uglify-js">uglify-js</a>
</pre>


Bundle your application:
<pre>
$(<a href="https://docs.npmjs.com/cli/bin">npm bin</a>)/browserify \
    -t hyperxify \
    -t babelify \
    -g uglifyify \
    -p bundle-collapser/plugin index.js | uglifyjs > bundle.js
</pre>

[Get this boilerplate](https://gist.github.com/jbucaran/48c1edb4fb0ea1aa5415b6686cc7fb45).

### Webpack

Install development dependencies:

<pre>
npm i -D \
    <a href="https://www.npmjs.com/package/hyperx">hyperx</a> \
    <a href="https://www.npmjs.com/package/webpack">webpack</a> \
    <a href="https://www.npmjs.com/package/transform-loader">transform-loader</a> \
    <a href="https://www.npmjs.com/package/babel-core">babel-core</a> \
    <a href="https://www.npmjs.com/package/babel-loader">babel-loader</a> \
    <a href="https://www.npmjs.com/package/babel-preset-es2015">babel-preset-es2015</a>
</pre>

Create a `.babelrc` file:
```js
{
    "presets": ["es2015"]
}
```

Create a `webpack.config.js` file:

```jsx
module.exports = {
    entry: "./index.js",
    output: {
        filename: "bundle.js",
    },
    module: {
        loaders: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: "babel-loader!transform-loader?hyperxify"
            }
        ]
    }
}
```

Bundle your application:
<pre>
$(<a href="https://docs.npmjs.com/cli/bin">npm bin</a>)/webpack -p
</pre>

[Get this boilerplate](https://gist.github.com/jbucaran/c6a6bdb5383a985cec6b0ae4ebe5a4b1).

### Rollup

Install development dependencies:

<pre>
npm i -D \
    <a href="https://www.npmjs.com/package/babel-preset-es2015-rollup">babel-preset-es2015-rollup</a> \
    <a href="https://www.npmjs.com/package/hyperx">hyperx</a> \
    <a href="https://www.npmjs.com/package/hyperxify">hyperxify</a> \
    <a href="https://www.npmjs.com/package/rollup">rollup</a> \
    <a href="https://www.npmjs.com/package/rollup-plugin-browserify-transform">rollup-plugin-browserify-transform</a> \
    <a href="https://www.npmjs.com/package/rollup-plugin-buble">rollup-plugin-buble</a> \
    <a href="https://www.npmjs.com/package/rollup-plugin-commonjs">rollup-plugin-commonjs</a> \
    <a href="https://www.npmjs.com/package/rollup-plugin-node-resolve">rollup-plugin-node-resolve</a> \
    <a href="https://www.npmjs.com/package/rollup-plugin-uglify">rollup-plugin-uglify</a>
</pre>

Create a `rollup.config.js` file:

```jsx
import buble from "rollup-plugin-buble"
import resolve from "rollup-plugin-node-resolve"
import uglify from "rollup-plugin-uglify"
import browserify from "rollup-plugin-browserify-transform"
import hyperxify from "hyperxify"
import cjs from "rollup-plugin-commonjs"

export default {
	moduleName: "window",
	plugins: [
		browserify(hyperxify),
		buble(),
		cjs(),
		resolve({
			module: false
		}),
		uglify()
	]
}
```

Bundle your application:
<pre>
$(<a href="https://docs.npmjs.com/cli/bin">npm bin</a>)/rollup -cf iife -i index.js -o bundle.js
</pre>

[Get this boilerplate](https://gist.github.com/jbucaran/fac2c3de24e5171596fb189f9c1feb8e).