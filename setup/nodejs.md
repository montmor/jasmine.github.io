---
layout: article
title: Node.js Setup
redirect_from: /edge/node.html
---

# Using Jasmine with node

The Jasmine node package contains helper code for developing and running Jasmine tests for node-based projects.

## Install

You can install Jasmine using npm, locally in your project and globally to use the CLI tool.

```sh
npm install jasmine

npm install -g jasmine
```

## Init a Project

Initialize a project for Jasmine by creating a spec directory and configuration json for you.

```sh
jasmine init
```

## Generate examples

Generate example spec and source files

```sh
jasmine examples
```

At this point you should be able to [write your first suite](/tutorials/your_first_suite.html)

## Configuration

Customize `spec/support/jasmine.json` to enumerate the source files and spec files you would like the
Jasmine runner to include. You may use dir glob strings.

`spec_dir` is used as a prefix for all `spec_files` and `helpers`.
Helpers are executed before specs.

```javascript
{
  // Spec directory path. Your spec_files must be relative to this path
  "spec_dir": "spec",
  // Array of filepaths (and globs) relative to spec_dir to include
  "spec_files": [
    "**/*[sS]pec.js"
  ],
  // Array of filepaths (and globs) relative spec_dir to include before jasmine specs
  "helpers": [
    "helpers/**/*.js"
  ],
  // Stop execution of a spec after the first expectation failure in it
  stopSpecOnExpectationFailure: false,
  // Run specs in semi-random order
  random: false
}
```

## Running tests

Once you have set up your `jasmine.json`, you can start Jasmine by running `jasmine` from the root of your project.

Pass a relative path to a spec file to the jasmine command to only execute specs in a single file.

```sh
jasmine

jasmine spec/appSpec.js
```

### CLI Options

#### `JASMINE_CONFIG_PATH=`
Specify a relative or absolute path to your configuration file. Can be used as an option or set as an environment variable.

```sh
JASMINE_CONFIG_PATH=spec/config/jasmine.json jasmine

jasmine JASMINE_CONFIG_PATH=spec/config/jasmine.json
```

#### `----no-color`
Turns off color in spec output

```sh
jasmine --no-color
```

#### `----filter=`
Only runs specs that match the given string

```sh
jasmine --filter="a spec name"
```

#### `----stop-on-failure=[true|false]`
 Stops execution of a spec after the first expectation failure when set to `true`

```sh
jasmine --stop-on-failure=true
```

#### `----random=[true|false]`
Tells jasmine to run specs in semi random order or not for this run, overriding `jasmine.json`

```sh
jasmine --random=true
```

#### `----seed=`
Sets the randomization seed if randomization is turned on

```sh
jasmine --seed=4321
```

## Using the library

If you want more granular control over the configuration, Jasmine can also be used as a library in your project.
This allows you to load multiple config files or control your configuration in different ways.

```javascript
var Jasmine = require('jasmine');
var jasmine = new Jasmine();
```

### Load configuration from a file or from an object.

```javascript
jasmine.loadConfigFile('spec/support/jasmine.json');

jasmine.loadConfig({
    spec_dir: 'spec',
    spec_files: [
        'appSpec.js',
        'requests/**/*[sS]pec.js',
        'utils/**/*[sS]pec.js'
    ],
    helpers: [
        'helpers/**/*.js'
    ]
});
```

### Custom onComplete

Optionally specify a custom `onComplete` callback. The callback is given a boolean of whether all of the specs passed or not. This is often used to message a status to task runners like grunt.

```javascript
jasmine.onComplete(function(passed) {
    if(passed) {
        console.log('All specs have passed');
    }
    else {
        console.log('At least one spec has failed');
    }
});
```

### Reporters

A ConsoleReporter is included if no other reporters are added.
You can configure the default reporter with `configureDefaultReporter`.
The default values are shown in the example.

```javascript
jasmine.configureDefaultReporter({
    // The `timer` passed to the reporter will determine the mechanism for seeing how long the suite takes to run.
    timer: new jasmine.jasmine.Timer(),
    // The `print` function passed the reporter will be called to print its results.
    print: function() {
        process.stdout.write(arguments);
    },
    // `showColors` determines whether or not the reporter should use ANSI color codes.
    showColors: true
});
```

You can add a custom reporter with `addReporter`. If you add a reporter through `addReporter`, the default ConsoleReporter will not be added.
Multiple reporters can be added.

```javascript
var CustomReporter = require('./myCustomReporter');
var customReporter = new CustomReporter();

jasmine.addReporter(customReporter);
```

### Run the tests

Calling `execute` will run the specs.

```javascript
jasmine.execute();
```

`execute` can optionally be called with a list of spec file paths to execute relative to your project root and a string to filter by spec name.

```javascript
jasmine.execute(['fooSpec.js'], 'a spec name');
```

### A simple example using the library

```javascript
var Jasmine = require('jasmine');
var jasmine = new Jasmine();

jasmine.loadConfigFile('spec/support/jasmine.json');
jasmine.configureDefaultReporter({
    showColors: false
});
jasmine.execute();
```
