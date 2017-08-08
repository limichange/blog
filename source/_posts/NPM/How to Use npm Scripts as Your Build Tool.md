## Class
From https://egghead.io/lessons/tools-run-npm-scripts-in-series

## Run the basic npm scripts

In this lesson we will examine two common scripts (start and test) that npm natively understands. We’ll define these scripts for our existing repository and show how you can run the scripts from the terminal.

```bash
# npm run test
$ npm t

# npm run start
$ npm start
```

## Create a custom npm script

A lot of the power behind npm scripts is creating custom scripts that are outside the basic set that npm natively understands. In this lesson we will install the ESLint node package, create a new eslint npm script, briefly discuss about environment variables and how npm knows where to find your binary, and then execute our custom script from the command line.

```json
{
  ...
  "scripts": {
    "eslint": "eslint --fix src"
  }
  ...
}
```

## Run npm scripts in series

After creating several npm script it becomes useful to run multiple scripts back-to-back in series. This is a nice feature because you can enforce that one script needs to complete before starting another one.

```json
{
  ...
  "scripts": {
    "eslint": "eslint --fix src",
    "test": "mocha test",
    "serve": "node index.js",
    "dev": "npm run eslint && npm run test && npm run serve"
  }
  ...
}
```

## Run npm scripts in parallel

In this lesson we will look at running several npm scripts in parallel. Sometimes you don’t need scripts to be run in series and switching them to run in parallel can increase performance since they are not blocking each other. At the end you need to add a wait command so they can be terminated with ^C

```json
{
  ...
  "scripts": {
    "eslint": "eslint --fix src",
    "test": "mocha test --watch",
    "serve": "node index.js",
    "dev": "npm run eslint & npm run test & npm run serve & wait"
  }
  ...
}
```

## Use a shorthand syntax for running multiple npm scripts with npm-run-all

Running multiple scripts in series or in parallel can become very verbose. Using a tool such as npm-run-all can help reduce the amount of overhead you have to type in order to get the same behavior.

```bash
$ npm i npm-run-all -D
```

```json
{
  ...
  "scripts": {
    "eslint": "eslint --fix src",
    "test": "mocha test --watch",
    "serve": "node index.js",
    "dev": "npm-run-all --parallel eslint test serve"
  }
  ...
}
```

## Run a set of similar npm scripts with a wildcard

In this lesson we will run a set of scripts that are grouped together with a wildcard using the npm-run-all node package. Using this technique can help you simplify and organize your scripts.

```json
{
  ...
  "scripts": {
    "eslint:js": "eslint --fix *.js",
    "eslint:css": "eslint --fix *.css",
    "eslint:css:dev": "eslint --fix dev/*.css",
    "eslint-all": "npm run eslint:**"
  }
  ...
}
```
