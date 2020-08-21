# jsplato

Visualize JavaScript source complexity with plato.
It supports `.js .jsx .vue`
Based on the older es5 plato, this is a port to `es6` and `eslint`

#The Report
![dank-es6-nugs](https://cloud.githubusercontent.com/assets/954596/18904556/3a81efea-8524-11e6-8588-ad8f5a51b001.PNG)

## Start in 3 steps.

1. Install.
   `npm install --save-dev jsplato`

2. Add.

```javascript
"scripts" : {
    "jsplato": "jsplato -r -d ./report src",
}
```

3. Run.
   `npm run jsplato`

## Installation

Install the module with: `npm install --save-dev jsplato`

## Usage

### From scripts

```js
//be sure and set your src, output, and any options.
let src = "./scripts/**/*.js";
let outputDir = "./artifacts/plato";

let platoArgs = {
  title: "example",
  eslint: {}
};

//you can use the reports in the callback.
function callback(reports) {
  let overview = plato.getOverviewReport(reports);

  let { total, average } = overview.summary;

  let output = `total
    ----------------------
    eslint: ${total.eslint}
    sloc: ${total.sloc}
    maintainability: ${total.maintainability}
    average
    ----------------------
    eslint: ${average.eslint}
    sloc: ${average.sloc}
    maintainability: ${average.maintainability}`;

  console.log(output);
}

//usage is plato.inspect
plato.inspect(src, outputDir, platoArgs, callback);
```

# Example Gulpfile

```js
let gulp = require("gulp");
let plato = require("jsplato");

let src = "./scripts/**/*.js";
let outputDir = "./artifacts/plato";

let lintRules = {
  rules: {
    indent: [2, "tab"],
    quotes: [2, "single"],
    semi: [2, "always"],
    "no-console": [1],
    curly: ["error"],
    "no-dupe-keys": 2,
    "func-names": [1, "always"]
  },
  env: {
    es6: true
  },
  globals: ["require"],
  parserOptions: {
    sourceType: "module",
    ecmaFeatures: {
      jsx: true,
      modules: true
    }
  }
};

let complexityRules = {};

let platoArgs = {
  title: "example",
  eslint: lintRules,
  complexity: complexityRules
};

function analysis() {
  return plato.inspect(src, outputDir, platoArgs);
}

gulp.task("analysis", analysis);
```

### From the commandline

```sh
Usage : jsplato [options] -d <output_dir> <input files>
  -h, --help
      Display this help text.
  -q, --quiet
      Reduce output to errors only
  -v, --version
      Print the version.
  -x, --exclude : String
      File exclusion regex
  -d, --dir : String *required*
      The output directory
  -r, --recurse
      Recursively search directories
  -l, --jshint : String
      Specify a jshintrc file for JSHint linting
  -t, --title : String
      Title of the report
  -D, --date : String
      Time to use as the report date (seconds, > 9999999999 assumed to be ms)
  -n, --noempty
      Skips empty lines from line count
  -e, --eslint : String
      Specify a eslintrc file for ESLint linting
```

**Example**

```shell
jsplato -r -d report src
```

> Note for Windows Users:
 If you are on Windows, you might want to put your glob in quotes if you use a tool such as cygwin, conemu or some other emulator, and you are also targeting files in directories, otherwise the emulator might incorrectly expand the glob before it is handled internally by es6-plato. For instance, if you want to use `/src/**/*.js` and the results are ignoring the root try `'./src/**/*.js'` instead.
>


![class functions, ya'll](https://cloud.githubusercontent.com/assets/954596/18904476/d1a57302-8523-11e6-85df-b474be8c59a8.PNG)

## Data sources

- Complexity from [typhonjs-escomplex](https://github.com/typhonjs-node-escomplex/typhonjs-escomplex)
- Lint data from [eslint](http://eslint.org/)

## Contributors

- [Jesse Harlin](https://github.com/the-simian)
- [Jarrod Overson](https://github.com/jsoverson)
- [Craig Davis](https://github.com/there4)
- [David Linse](https://github.com/davidlinse)

## Release History

| version     | update                                                                                           |
| ----------- | ------------------------------------------------------------------------------------------------ |
| 1.0.2-alpha | Project works with es6 and eslint                                                                |
| 1.0.6-alpha | Use typhonjs-escomplex                                                                           |
| 1.0.0       | Class methods parsed and evaluated correctly                                                     |
| 1.0.2       | Fix error when no callback supplied                                                              |
| 1.0.5       | Update dependencies; fix lodash; add summary display link                                        |
| 1.0.7       | Default complexity to 1-100 not 1-177, this can be overridden in the complexity object settings. |
| 1.0.8       | Fixes to eslint allowing for plugin usage.                                                       |
| 1.0.9       | Update dependencies to latest versions                                                           |
| 1.0.13      | Fix templates to work in some CI envs + add jsx support                                          |
| 1.0.14      | update dependencies in package.json                                                              |
| 1.0.15      | update dependencies in package.json                                                              |
| 1.0.16      | switch to globby, address Linux line endings                                                     |
| 1.0.17      | Explicitly add eslint-plugin-react and update the dependencies                                   |
| 1.0.18      | Offer eslintrc option in cli and update documentation, update dependencies too                   |
| 1.1.15      | Update the dependencies and remove Grunt, for now since it was insecure dependency               |
| 1.1.16      | Update eslint to 5.14.0                                                                          |
| 1.2.0       | Update eslint, globby, lodash, typhon-complex and others                                         |
| 1.2.1       | reverts typhon-complex for now, see issue #95                                                    |
| 1.2.2       | reverts globby, 10 doesn't by default handle windows slashes                                     |
| 1.2.3       | updates eslint and globby                                  |
| 1.2.4       | updates lodash                                |

## base on above work

| version     | update                                                                                           |
| ----------- | ------------------------------------------------------------------------------------------------ |
| 0.0.1       | supports .vue                                 |
| 0.0.3       | supports .ts and .tsx                                 |

## About

This is currently a reimplementation of the older plato, and started as a fork from https://github.com/deedubs/es6-plato, but has since been heavily modified.
After seeing it was unpublished on npm and also wanting to add more features, I Asked if it [would be alright for me to publish and continue the work.](https://github.com/deedubs/es6-plato/issues/4)
This project uses eslint, not jshint for default linting.

I have switched to the [typhon-js](https://github.com/typhonjs-node-escomplex/typhonjs-escomplex) module since it properly parses classes.

## License

Copyright (c) 2012 Jesse Harlin
Licensed under the MIT license.
