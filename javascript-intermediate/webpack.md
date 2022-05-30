# Webpack
- Webpack can be used for compiling Sass to CSS, minifying code, optimizing images, etc. 

- Webpack enables you to included npm modules in frontend code (using the `import` syntax). It'll include the npm stuff that you actually use within your minified js bundle. 

- Before webpack tools such as grunt or gulp were used to process assets.

- Generate a webpack configuration file with the webpack cli: `npx webpack-cli init`.

- When you run `npx webpack` the default behavior is to take the script at `src/index.js` as the entry point, and generate `dist/main.js` as the output.

  - You can also add webpack to your `package.json` scripts and then run it with `npm run`:

    ```
    "scripts": {
    	"build": "webpack"
    }
    ```


- Putting webpack in development mode will enable source maps. This way you can debug your code using Chrome DevTools and put breakpoints on the original src file.

  Add this to your webpack config file:

  ```
  {
    mode: "development"
    // the rest of your webpack.config.js
  }
  ```

  Or add this option to your webpack CLI command: `webpack -d --mode development`

  