## ECMAScript info 
- **ES6 and ES2015 are the same thing**: a significant JS update that happened in 2015. Prior to its release all major JS versions were named ES1, ES2, etc. With its release the naming changed to include the year in the name.
	- ES2016 is basically ES7, ES2017 is basically ES8
- **ECMAScript** is a standard. **JavaScript** is the most popular _implementation_ of that standard. Other implementations include: [SpiderMonkey](https://en.wikipedia.org/wiki/SpiderMonkey) (Firefox), [V8](https://en.wikipedia.org/wiki/Chrome_V8) (Chrome), and [ActionScript](https://en.wikipedia.org/wiki/ActionScript).
	- ES is just an abbreviation for ECMAScript and updates to ES are updates to the overall JS standard.
	- Browsers don't implement all of the new standard at the same time. At the current time all modern browsers (Chrome, Firefox, Safari and Edge) support _all_ of ES6, and _most_ of ES7, but older browsers (various versions of Internet Explorer for instance) do not.
- Sometimes people write ES.Next to refer to the next (unreleased) version of ES. 
- `let`, `const`, classes, destructuring, rest params, for-of loops are [all things that were introduced with ES2015]([lukehoban/es6features: Overview of ECMAScript 6 features (github.com)](https://github.com/lukehoban/es6features)).

## Babel
- [Babel](http://babeljs.io/) is a tool that takes your modern JavaScript code and **transpiles** it to code that older browsers can understand.
	- It can also transpile JSX, which is basically an extension of the JS language that you use for React.
- [Great intro tutorial]([What is @babel/preset-env and why do I need it? | blog.jakoblind.no](https://blog.jakoblind.no/babel-preset-env/))

### Babel Loader
- Add it to you webpack configuration with [Babel Loader]([babel/babel-loader: 📦 Babel loader for webpack (github.com)](https://github.com/babel/babel-loader)). This loads ES2015+ code and transpiles to ES5 using [Babel](https://babeljs.io/).
	- You can set up babel-loader automatically if you use the webpack cli to create your `webpack.config.js`.
	- [Webpack loaders]([Loaders | webpack](https://webpack.js.org/loaders/)) let you preprocess files before they are bundled. Babel loader is an example of a webpack loader. There are other loaders for SASS, for example.
	- To install Babel Loader (and webpack): `npm install -D babel-loader @babel/core @babel/preset-env`
	- Make sure that `babel-loader` is added as a module to your webpack config file:
``` javascript
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: [
            ['@babel/preset-env', { targets: "defaults" }]
          ]
          /* other babel options here */
        }
      }
    },
    /* ... other modules here */
  ]
}

```

### Babel CLI
- You can also use Babel with a CLI: `npm install --save-dev @babel/core @babel/cli`
	- This lets you run babel on specific JS script with `npx babel script.js`

### Presets vs Plugins
- Babel plugins and presets are downloaded with npm, but for babel to actually use them you need to specify them in a `.babelrc` file.
	- Babel doesn't do anything by default. There is a different plugin for transpiling each ES6 feature, and a preset is essentially a collection of plugins. 
	- Each plugin is it’s own NPM library. So for every plugin you want to install, you have to install a new NPM library or you can install a preset that includes a bunch of them.

### Env Preset
- `@babel/preset-env` is a preset, which is basically a bunch of configeration features (aka plugins) for what babel should do. 
	- You need to specify the targets (aka the browsers you want to support). Eg. 
	- `babel-preset-env` is an older version from before preset-env was moved to the main babel repo.
- The unique selling point with babel-preset-env is that you can define what browsers you support, instead of just transpiling everything to legacy javascript.
	- You do this with a `.browserslistrc` file where you follow a [standard](https://github.com/browserslist/browserslist#config-file) to specify the browsers that you want to support.
	- Or you can specify the browsers directly in the `package.json` according to the same standard. Just add a `browserslist` array to the main object: 
``` javascript
  "browserslist": [
    "defaults", // just using the defaults is a good configuration for most users
    "not IE 11",
    "maintained node versions"
  ]  
```
- You can get a list of all browsers supported by your browserlist config by running following command




