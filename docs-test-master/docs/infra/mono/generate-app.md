---
title: Generators
---

## Getting started

`mono generate` is the command line utility allowing the creation of applications and libraries, adding and updateing CI/CD.

There’s a list of [available generators](#available-generators) and its easy to [create a new one](#creating-a-generator) to match any workflow.

### Basic scaffolding

We believe in the composability nature of the generators. Thereby, each need will be addressed by a dedicated generator (each will be used depending of the options you selected or not).

Run in your terminal (you’ll need to have `mono` CLI set up on your machine):

```sh
mono generate
```

![mono generate](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-generate.gif)

### Incremental scaffolding

`mono generate` scaffolding complex frameworks is likely to invoke additional generators to scaffold different parts of a project. These generators could be run separately.

Take Shuvi app with CI/CD as an example. Instead of generating everything at once, you might want to split the work (set up CI/CD, then scaffold a Shuvi app):

```sh
mono generate --type cicd
mono generate --type shuvi
```

To see which options are available, use the `help` command:

```sh
mono generate --help
```

### Available generators

#### Caviar application

```sh
mono generate --type caviar
```

![mono generate caviar](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-generate-caviar.gif)

#### Shuvi application

```sh
mono generate --type shuvi
```

![mono generate shuvi](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-generate-shuvi.gif)

#### CI/CD

```sh
mono generate --type cicd
```

![mono generate cicd](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-generate-cicd.gif)

#### JavaScript or TypeScript library

```sh
mono generate --type lib
```

![mono generate lib](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-generate-lib.gif)

#### Generator

```sh
mono generate --type generator
```

![mono generate generator](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-generate-generator.gif)
![mono generate sub-generator](https://static.devfdg.net/static/mono-static/docs-ui/img/mono-generate-sub-generator.gif)

## Creating a generator

We use [Yeoman](https://yeoman.io/) – generic language agnostic scaffolding system. It by itself doesn’t make any decisions. Every decision is made by generators, which are basically plugins in the Yeoman environment.

### Writing your own generator

We built a [`generator-generator`](#generator) to help developers get started with their own generator. Feel free to use it to bootstrap your own generator or sub-generator once you understand the below concepts.

#### Organizing your generator

First, create a folder in `libs/` within which you’ll write your generator. This folder must be named `generator-${name}` (where `${name}` is the name of your generator).

Once inside your generator folder, create a `package.json` file. This file is a Node.js module manifest. For example:

```json
{
  "name": "@binance/generator-${name}",
  "version": "0.0.1",
  "description": "Yeoman generator to scaffold a ${name}",
  "files": [
    "generators"
  ],
  "repository": {
    "type": "git",
    "url": "https://git.toolsfdg.net/mono/mono.git"
  },
  "keywords": [
    "@binance/generator-${name}",
    "yeoman-generator",
    "yeoman",
    "generator",
    "${name}"
  ],
  "license": "UNLICENSED",
  "dependencies": {
    "yosay": "^2.0.2"
  }
}
```

#### Folder tree

Yeoman’s functionality is dependent on how you structure your directory tree. Each sub-generator is contained within its own folder.

The default generator used when you call `mono generate --type ${name}` is the `app` generator. This must be contained within the `app/` directory.

Sub-generators, used for [Composability](#composability), are stored in folders named exactly like the sub command.

In an example project, a directory tree could look like this:

```sh
├───BUILD.bazel
├───deps.bzl
├───OWNERS
├───package.json
└───generators/
    ├───app/
    │   └───index.js
    └───router/
        └───index.js
```

#### Extending generator

Once you have this structure in place, it’s time to write the actual generator.

We offer base generators which you can extend to implement your own behavior. These base generators will add most of the functionalities you’d expect to ease your task. Each generator is responsible for its own scope:

- `AppsGenerator` is responsibe for `apps/` derectory
- `InfraGenerator` is responsibe for `infra/` derectory
- `LibsGenerator` is responsibe for `libs/` derectory

You could find them in the [Class Diagram](#base-generators) below.

For example, here’s how you extend the base generator:

```js
const { AppsGenerator } = require('@binance/generators')

module.exports = class extends AppsGenerator {}
```

#### Adding your own functionality

Every method added to the prototype is run once the generator is called–and usually in sequence. But, some special method names will trigger a specific run order. To find out more, please read about [the generator runtime context](https://yeoman.io/authoring/running-context.html).

### User Interactions

Yeoman provides a set of user interface element abstractions. You'd better use those abstractions when interacting with end users. Using other ways will probably prevent your generator from running correctly in different Yeoman tools.

#### Options

Usually, we receive options from [Higher-Order-Generators](#execution-example).

To notify the system that we expect an option, we use the `this.option()` method. This method accepts a `name` (String) and an optional hash of options.

The name value will be used to retrieve the option at the matching key `this.options[name]`.

The options hash (the second argument) accepts multiple key-value pairs:

- `desc` Description for the option
- `type` Either Boolean, String or Number (can also be a custom function receiving the raw string value and parsing it). For an array please use the `Array.from` function
- `default` Default value

Here is an example:

```js
module.exports = class extends AppsGenerator {
  constructor(args, opts) {
    super(args, opts)

    this.option('ts', { type: Boolean })
    this.option('amount', { type: Number })
    this.option('username', { type: String })
    this.option('configs', { type: Array.from })

    // And you can then access them later; e.g.
    this.scriptSuffix = this.options.ts ? '.ts' : '.js'
  }
}
```

#### Prompts

Prompts are the main way a generator interacts with a user. The prompt module is provided by [Inquirer.js](https://github.com/SBoudrias/Inquirer.js) and you should refer [to its API](https://github.com/SBoudrias/Inquirer.js) for a list of available prompt options.

All the `prompting` methods are asynchronous and return a promise. You’ll need to return the promise from your task in order to wait for its completion before running the next one. ([learn more about asynchronous task](https://yeoman.io/authoring/running-context.html))

```js
module.exports = class extends AppsGenerator {
  constructor(args, opts) {
    super(args, opts)

    this.option('cool', { type: Boolean })
  }

  prompting() {
    return this.prompt([
      {
        when: !this.options.cool,
        type: "confirm",
        name: "cool",
        message: "Would you like to enable the Cool feature?",
      },
    ])
  }
}
```

Note here that we use the [`prompting` queue](https://yeoman.io/authoring/running-context.html) to ask for feedback from the user.

#### Using user answers at a later stage

A very common scenario is to use the user answers at a later stage, e.g. in [`writing` queue](https://yeoman.io/authoring/running-context.html). This can be easily achieved by reading the user `answers` from `this` context:

```js
module.exports = class extends AppsGenerator {
  constructor(args, opts) {
    super(args, opts)

    this.option('cool', { type: Boolean })
  }

  prompting() {
    return this.prompt([
      {
        when: !this.options.cool,
        type: "confirm",
        name: "cool",
        message: "Would you like to enable the Cool feature?",
      },
    ])
  }

  writing() {
    this.log("cool feature", this.answers.cool); // user answer `cool` used
  }
}
```

#### Outputting Information

Outputting information is handled by the `this.log` module.

The main method you’ll use is simply `this.log`. It takes a string and outputs it to the user; basically it mimics `console.log()` when used inside of a terminal session. You can use it like so:

```js
module.exports = class extends AppsGenerator {
  myAction() {
    this.log("Something has gone wrong!");
  }
}
```

### Composability

> Composability is a way to combine smaller parts to make one large thing. Sort of like [Voltron®](https://static.devfdg.net/static/mono-static/docs-ui/img/voltron.gif)

Yeoman offers multiple ways for generators to build upon common ground. There’s no sense in rewriting the same functionality, so an API is provided to use generators inside other generators.

Composability is initiated when a generator decides to compose itself with another generator.

#### `this.composeWith()`

The `composeWith` method allows the generator to run side-by-side with another generator (or subgenerator). That way it can use features from the other generator instead of having to do it all by itself.

When composing, don’t forget about [the running context and the run loop](https://yeoman.io/authoring/running-context.html). On a given priority group execution, all composed generators will execute functions in that group. Afterwards, this will repeat for the next group. Execution between the generators is the same order as `composeWith` was called.

`composeWith` takes two parameters:

1. `generatorPath` A full path pointing to the generator you want to compose with (usually using `require.resolve()`)
2. `options` An Object containing options to pass to the composed generator once it runs

```js
this.composeWith(require.resolve('@binance/generator-cicd/generators/app'), { ...this.answers })
```

Or if a generator is a subgenerator within the same package, you cold simply use a relative path:

```js
this.composeWith(require.resolve('../subgenerator'), { ...this.answers })
```

`require.resolve()` returns the path from where Node.js would load the provided module.

Even though it is not an encouraged practice, you can also pass a generator namespace to `composeWith`. In that case, Yeoman will try to find that generator installed as a `dependencies`.

```js
this.composeWith('@binance/lib:app', { ...this.answers })
// which tantamounts
this.composeWith(require.resolve('@binance/generator-lib/generators/app'), { ...this.answers })
```

#### Execution example

```js
// In libs/generator-example/webapp/index.js
module.exports = class WebappGenerator extends AppsGenerator {
  constructor(args, opts) {
    super(args, opts)

    this.option('title', { type: String })
  }

  prompting() {
    this.log(`prompting - ${this.options.title}`)
  }

  writing() {
    this.log(`writing - ${this.options.title}`)
  }
}

// In libs/generator-example/test/index.js
module.exports = class TestGenerator extends AppsGenerator {
  constructor(args, opts) {
    super(args, opts)

    this.option('title', { type: String })
  }

  prompting() {
    this.log(`prompting - ${this.options.title}`)
  }

  writing() {
    this.log(`writing - ${this.options.title}`)
  }
}

// In libs/generator-example/app/index.js
module.exports = class Generator extends AppsGenerator {
  initializing() {
    this.composeWith(require.resolve('../webapp'), { title: 'webapp' })
    this.composeWith(require.resolve('../test'), { title: 'test' })
  }
}
```

This will result in:

```sh
prompting - webapp
prompting - test
writing - webapp
writing - test
```

### Managing Dependencies

Once you’ve run your generators, you’ll often want to run [Yarn](https://yarnpkg.com/) to install any additional dependencies your generators require.

Yeoman provided installation helpers will automatically schedule the installation to run once as part of the `install` queue. If you need to run anything after they’ve run, use the `end` queue.

You just need to call `this.yarnInstall()` to launch the installation. Yeoman will ensure the `yarn install` command is only run once even if it is called multiple time by multiple generators.

```js
module.exports = class extends LibsGenerator {
  install() {
    this.yarnInstall()
  }
}
```

### Interacting with the file system

Yeoman file utilities are based on the idea you always have two location contexts on disk. These contexts are folders your generator will most likely read from and write to.

#### Destination context

The first context is the destination context. The destination is the folder in which Yeoman will be scaffolding a new application or library.

The destination context is defined in the `routing` queue. Please, set the _destination path_ right after the `prompting` queue:

```js
const { AppsGenerator } = require('@binance/generators')

module.exports = class extends AppsGenerator {
   prompting() {
    return this.prompt()
  }

  routing() {
    this.route()
  }
}
```

The same for libs:

```js
const { LibsGenerator } = require('@binance/generators')

module.exports = class extends LibsGenerator {
  prompting() {
    return this.prompt()
  }

  routing() {
    this.route()
  }
}
```

This will seth _destination path_ to `apps/${name}` or `libs/${name}` folder respectively.

#### Template context

The template context is the folder in which you store your template files. It is usually the folder from which you’ll read and copy.

The template context is defined as `./templates/`.

#### An “in memory” file system

Yeoman is very careful when it comes to overwriting users files. Basically, every write happening on a pre-existing file will go through a conflict resolution process. This process requires that the user validate every file write that overwrites content to its file.

This behaviour prevents bad surprises and limits the risk of errors. On the other hand, this means every file is written asynchronously to the disk.

As asynchronous APIs are harder to use, Yeoman provide a synchronous file-system API where every file gets written to an [in-memory file system](https://github.com/sboudrias/mem-fs) and are only written to disk once when Yeoman is done running.

This memory file system is shared between all [composed generators](#composability).

#### File utilities

Generators expose all file methods on `this.fs`, which is an instance of [mem-fs editor](https://github.com/sboudrias/mem-fs-editor) - make sure to check the its documentation for all available methods.

It is worth noting that although `this.fs` exposes `commit`, you should not call it in your generator. Yeoman calls this internally after the conflicts stage of the run loop.

##### Example: Copying template files

Here’s an example where we’d want to copy and process template files.

Given the content of `./templates/package.json.ejs` is:

```json
{
  "name": "<%= name %>",
  "version": "0.0.1",
}
```

We’ll then use the `copyTemplates` method to copy the files while processing the content as a template. `copyTemplates` is using [ejs template syntax](https://ejs.co/).

```js
const { AppsGenerator } = require('@binance/generators')

module.exports = class extends AppsGenerator {
  routing() {
    this.route()
  }

  writing() {
    this.copyTemplates(['package.json.ejs'], { name: 'test-ui' })
  }
}
```

Once the generator is done running, `apps/test-ui/package.json` will contain:

```json
{
  "name": "test-ui",
  "version": "0.0.1",
}
```

A very common scenario is to use user answers after the [prompting stage](#user-interactions). They are used for templating automatically:

```js
const { AppsGenerator } = require('@binance/generators')

module.exports = class extends AppsGenerator {
  constructor(args, opts) {
    super(args, opts)

    this.option('name', { type: String })
  }

  prompting() {
    return this.prompt([
      {
        when: !this.options.name,
        type: "input",
        name: "name",
        message: "Your project name",
      },
    ])
  }

  routing() {
    this.route()
  }

  writing() {
    this.copyTemplates(['package.json.ejs'])
  }
}
```

##### Example: Copying templates by pattern

Here’s an example where we’d want to copy and process template files by pattern.

Given the content of `./templates/package.json.ejs` is:

```json
{
  "name": "<%= name %>",
  "version": "0.0.1",
}
```

We’ll then use the `copyTemplatesByPattern` method to copy the files by pattenr while processing the content as a template. `copyTemplatesByPattern` is using [ejs template syntax](https://ejs.co/). It also uses use user answers after the [prompting stage](#user-interactions) automatically:

```js
const { AppsGenerator } = require('@binance/generators')

module.exports = class extends AppsGenerator {
  constructor(args, opts) {
    super(args, opts)

    this.option('name', { type: String })
  }

  prompting() {
    return this.prompt([
      {
        when: !this.options.name,
        type: "input",
        name: "name",
        message: "Your project name",
      },
    ])
  }

  routing() {
    this.route()
  }

  writing() {
    this.copyTemplatesByPattern('**/*')
  }
}
```

### Unit Testing

It is important to keep your tests simple and easily editable.

Usually the best way to organize your tests is to separate each generator and sub-generator into its own `describe` block. Then, add a `describe` block for each option your generator accepts. And then, use an `it` block for each assertion (or related assertion).

In code, you should end up with a structure similar to this:

```js
describe('UT shuvi:app', () => {
  describe('generator', () => {
    it('generates a project from options', () => {})
    it('generates a project from prompts', () => {})
  })
})
```

Yeoman provide test helpers methods. They’re contained inside the `yeoman-test` package.

```js
const helpers = require('yeoman-test')
```

You can check [the full helpers API here](https://github.com/yeoman/yeoman-test).

The most useful method when unit testing a generator is `helpers.run()`. This method will return a [`RunResult`](https://github.com/yeoman/yeoman-test/blob/main/lib/run-result.js) instance on which you can call assertions using mem-fs.

```js
const fs = require('fs')
const path = require('path')
const helpers = require('yeoman-test')

const monoTestPath = path.join(__dirname, Math.random().toString(36))

describe('UT shuvi:app', () => {
  const name = 'test'
  const confirmed = true

  beforeEach(() => {
    fs.mkdirSync(monoTestPath)
  })

  afterEach(() => {
    fs.rmdirSync(monoTestPath, { recursive: true })
  })

  describe('generator', () => {
    it('generates smth', () =>
      // The object returned acts like a promise, so return it to wait until the process is done
      helpers
        .create(path.join(__dirname, '../../generators/app'))
        .cd(monoTestPath)           // Run the test inside a non temporary dir
        .withOptions({ name })      // Mock options passed in
        .withPrompts({ confirmed }) // Mock the prompt answers
        .run()                      // Run the environment
        .then(runResult => {        // Assert something about the generator
          // runResult.assertFile('file.txt')
          // runResult.assertNoFile('file.txt')
          // runResult.assertFileContent('file.txt', 'content')
          // runResult.assertEqualsFileContent('file.txt', 'content')
          // runResult.assertNoFileContent('file.txt', 'content')
          // runResult.assertJsonFileContent('file.txt', {})
          // runResult.assertNoJsonFileContent('file.txt', {})
        }))
    })
  })
})
```

Another useful method when unit testing a generator is `helpers.build()`. This method will return a [`RunContext`](https://github.com/yeoman/yeoman-test/blob/main/lib/run-context.js) instance on which you can access the environment and your generator.

```js
const fs = require('fs')
const path = require('path')
const helpers = require('yeoman-test')

const monoTestPath = path.join(__dirname, Math.random().toString(36))

describe('UT shuvi:app', () => {
  const name = 'test'
  const confirmed = true

  beforeEach(() => {
    fs.mkdirSync(monoTestPath)
  })

  afterEach(() => {
    fs.rmdirSync(monoTestPath, { recursive: true })
  })

  describe('generator', () => {
    it('generates smth', () =>
      helpers
        .create(path.join(__dirname, '../../generators/app'))
        .cd(monoTestPath)
        .withOptions({ name })
        .withPrompts({ confirmed })
        .build(runContext => {      // Instantiate Environment/Generator
          // runContext.env         // Do something with the environment
          // runContext.generator   // Do something with the generator
        })
        .run() 
        .then(runResult => {}))
    })
  })
})
```

If your generator calls `composeWith()`, you may want to mock those dependent generators. Using `withGenerators()`, pass in array of arrays that use `createDummyGenerator()` as the first item and a namespace for the mocked generator as a second item:

```js
it('generates smth', () =>
  helpers
    .create(path.join(__dirname, '../../generators/app'))
    .cd(monoTestPath)
    .withGenerators([[helpers.createDummyGenerator(), '@binance/cicd:app']])
    .withPrompts({ name, confirmed })
    .run() 
    .then(runResult => {}))
```

You could also assert that your mocked generator has been called by using `withMockedGenerators()`. Pass in array of namespaces for the mocked generators:

```js
it('generates smth', () =>
  helpers
    .create(path.join(__dirname, '../../generators/app'))
    .cd(monoTestPath)
    .withMockedGenerators(['@binance/cicd:app'])
    .withPrompts({ name, confirmed })
    .run()
    .then(runResult => {
      expect(runResult.mockedGenerators['@binance/cicd:app'].calledOnce)
    }))
```

### Running Generators

Mono CLI is responsible for running generators. Before running your new generetor you will need to add it to the [list of generators in `tools/yo/BUILD.bazel`](https://git.toolsfdg.net/mono/mono/blob/master/tools/yo/BUILD.bazel).

You could also update the documentation of [`mono generate` in `tools/bin/mono-generate`](https://git.toolsfdg.net/mono/mono/blob/master/tools/bin/mono-generate)

## Generators Structure

### Class Diagram

import Mermaid from '@theme/Mermaid'

#### Base Generators

`BaseGenerator` extends `YeomanGenerator` by adding additional functionality to ease our tasks.

`AppsGenerator`, `InfraGenerator`, `LibsGenerator` are helper generators that are responsible for `apps/`, `infra/`, `libs/` directories respectively. We extend this Generators to implement our own behavior within these directories.

<Mermaid chart={`
    classDiagram
    EventEmitter <|-- YeomanGenerator
    MemFsEditor --o YeomanGenerator
    YeomanGenerator <|-- BaseGenerator
    BaseGenerator <|-- AppsGenerator
    BaseGenerator <|-- InfraGenerator
    BaseGenerator <|-- LibsGenerator
    class BaseGenerator {
        prompt(additionalPrompts) Promise
        route(...paths)
        confirm(title, tpl, context) Promise
        copyTemplate(from, to, context)
        copyTemplates(templates, context)
        copyTemplatesByPattern(templatesPattern, destinationDirectory, context)
        getYamlAnchor(doc, name) YAMLMap
        readYaml(filepath, defaults) YAML.Document
        updateYaml(filepath, update)
        writeYaml(filepath, doc)
    }
    class AppsGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    class InfraGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    class LibsGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    link EventEmitter "https://nodejs.org/api/events.html#events_events" "Click to see spec"
    link MemFsEditor "https://github.com/SBoudrias/mem-fs-editor" "Click to see spec"
    link YeomanGenerator "https://github.com/yeoman/generator" "Click to see spec"
`}/>

#### Master Generator

`MasterGenerator` implements the composability nature of the generators. Thereby, each need will be addressed by a dedicated generator inside the `composing` queue (each will be used depending of the options you selected or not).

[`mono generate`](#basic-scaffolding) runs `MasterGenerator`.

<Mermaid chart={`
    classDiagram
    BaseGenerator <|-- MasterGenerator
    class MasterGenerator {
        initializing()
        prompting() Promise
        composing()
    }
`}></Mermaid>

#### Apps Generators

`CaviarGenerator` is a generator for Caviar framework that provides different samples:

- `HelloCaviarGenerator` is a sub-generator that will generate the _Hello World_ sample.

[`mono generate --type caviar`](#caviar-application) runs `CaviarGenerator`.

`ShuviGenerator` is a generator for Shuvi framework that provides different samples:

- `JsShuviGenerator` is a sub-generator that will generate the _Hello World_ sample.

[`mono generate --type shuvi`](#shuvi-application) runs `ShuviGenerator`.

<Mermaid chart={`
    classDiagram
    BaseGenerator <|-- MasterGenerator
    BaseGenerator <|-- AppsGenerator
    AppsGenerator <|-- CaviarGenerator
    MasterGenerator o-- CaviarGenerator
    AppsGenerator <|-- HelloCaviarGenerator
    CaviarGenerator o-- HelloCaviarGenerator
    AppsGenerator <|-- ShuviGenerator
    MasterGenerator o-- ShuviGenerator
    AppsGenerator <|-- JsShuviGenerator
    ShuviGenerator o-- JsShuviGenerator
    class CaviarGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class HelloCaviarGenerator {
        String _templatesPattern
        initializing()
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
    class ShuviGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class JsShuviGenerator {
        String _templatesPattern
        initializing()
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
`}/>

#### Infra Generators

`CicdGenerator` is a CI/CD generator for different frameworks:

- `WebappCicdGenerator` is a sub-generator that will generate CI/CD for webapps.

[`mono generate --type cicd`](#cicd) runs `CicdGenerator`.

<Mermaid chart={`
    classDiagram
    BaseGenerator <|-- InfraGenerator
    BaseGenerator <|-- MasterGenerator
    InfraGenerator <|-- CicdGenerator
    MasterGenerator o-- CicdGenerator
    InfraGenerator <|-- WebappCicdGenerator
    CicdGenerator o-- WebappCicdGenerator
    class InfraGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    class CicdGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class WebappCicdGenerator {
        String[] _files
        String[] _templates
        prompting() Promise
        routing()
        confirming() Promise
        _getUploadStaticToS3() String
        _createPipelineRunSpecYamlNode(doc, options) Node
        _createPresubmitYamlNode(doc) Node
        _createPostsubmitYamlNode(doc) Node
        writing()
    }
`}/>

#### Libs Generators

`LibGenerator` is a generator for a library:

- `JsLibGenerator` is a sub-generator that will generate a JavaScript library
- `TsLibGenerator` is a sub-generator that will generate a TypeScript library.

[`mono generate --type lib`](#javascript-or-typescript-library) runs `LibGenerator`.

<Mermaid chart={`
    classDiagram
    BaseGenerator <|-- LibsGenerator
    BaseGenerator <|-- MasterGenerator
    LibsGenerator <|-- LibGenerator
    MasterGenerator o-- LibGenerator
    LibsGenerator <|-- JsLibGenerator
    LibGenerator o-- JsLibGenerator
    LibGenerator o-- TsLibGenerator
    LibsGenerator <|-- TsLibGenerator
    class LibsGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    class LibGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class JsLibGenerator {
        String _templatesPattern
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
    class TsLibGenerator {
        String[] _templates
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
`}/>

#### Generator Generators

`GeneratorGenerator` is a generator for generators that helps to get started with a new generator. Depending on user aunswers, the generator might call its sub-generators:

- `ClassGeneratorGenerator` is responsible for updating the list of base generators in `/libs/generators/constants/type.js`. A new class (if was created) will be available in [`MasterGenerator`](#master-generator)
- `FrameworkGeneratorGenerator` is responsible for updating the list of frameworks in `/libs/generators/constants/framework.js`. A new framework (if was created) will be available in [`MasterGenerator`](#master-generator)
- `YoGeneratorGenerator` is responsible for addin the new generator to the list in `/tools/yo/generators.bzl`. It allows to execute `mono generate --type ${new_generator}` right away.

[`mono generate --type generator`](#generator) runs `GeneratorGenerator`.

<Mermaid chart={`
    classDiagram
    BaseGenerator <|-- GeneratorGenerator
    BaseGenerator <|-- MasterGenerator
    BaseGenerator <|-- ClassGeneratorGenerator
    MasterGenerator o-- GeneratorGenerator
    GeneratorGenerator o-- ClassGeneratorGenerator
    GeneratorGenerator o-- FrameworkGeneratorGenerator
    GeneratorGenerator o-- YoGeneratorGenerator
    BaseGenerator <|-- FrameworkGeneratorGenerator
    BaseGenerator <|-- YoGeneratorGenerator
    class GeneratorGenerator {
        String[] _templates
        initializing()
        prompting() Promise
        configuring()
        composing()
        routing()
        confirming() Promise
        writing()
        install()
    }
    class ClassGeneratorGenerator {
        String _typeJs
        String _indexJs
        String[] _templates
        initializing()
        prompting() Promise
        configuring()
        routing()
        confirming() Promise
        _updateTypeJs()
        _updateIndexJs()
        writing()
    }
    class FrameworkGeneratorGenerator {
        String _frameworkJs
        initializing()
        prompting() Promise
        configuring()
        routing()
        confirming() Promise
        writing()
    }
    class YoGeneratorGenerator {
        String _yoBuildBazel
        initializing()
        prompting() Promise
        routing()
        confirming() Promise
        writing()
    }
`}/>

#### Full Class Diagram

<Mermaid chart={`
    classDiagram
    EventEmitter <|-- YeomanGenerator
    MemFsEditor --* YeomanGenerator
    YeomanGenerator <|-- BaseGenerator
    BaseGenerator <|-- MasterGenerator
    BaseGenerator <|-- GeneratorGenerator
    BaseGenerator <|-- ClassGeneratorGenerator
    BaseGenerator <|-- FrameworkGeneratorGenerator
    BaseGenerator <|-- YoGeneratorGenerator
    BaseGenerator <|-- AppsGenerator
    BaseGenerator <|-- InfraGenerator
    BaseGenerator <|-- LibsGenerator
    AppsGenerator <|-- CaviarGenerator
    MasterGenerator o-- CaviarGenerator
    AppsGenerator <|-- HelloCaviarGenerator
    CaviarGenerator o-- HelloCaviarGenerator
    AppsGenerator <|-- ShuviGenerator
    MasterGenerator o-- ShuviGenerator
    AppsGenerator <|-- JsShuviGenerator
    ShuviGenerator o-- JsShuviGenerator
    InfraGenerator <|-- CicdGenerator
    MasterGenerator o-- CicdGenerator
    CicdGenerator o-- WebappCicdGenerator
    InfraGenerator <|-- WebappCicdGenerator
    MasterGenerator o-- LibGenerator
    LibsGenerator <|-- LibGenerator
    LibGenerator o-- JsLibGenerator
    LibsGenerator <|-- JsLibGenerator
    LibGenerator o-- TsLibGenerator
    LibsGenerator <|-- TsLibGenerator
    MasterGenerator o-- GeneratorGenerator
    GeneratorGenerator o-- ClassGeneratorGenerator
    GeneratorGenerator o-- FrameworkGeneratorGenerator
    GeneratorGenerator o-- YoGeneratorGenerator
    class BaseGenerator {
        prompt(additionalPrompts) Promise
        route(...paths)
        confirm(title, tpl, context) Promise
        copyTemplate(from, to, context)
        copyTemplates(templates, context)
        copyTemplatesByPattern(templatesPattern, destinationDirectory, context)
        getYamlAnchor(doc, name) YAMLMap
        readYaml(filepath, defaults) YAML.Document
        updateYaml(filepath, update)
        writeYaml(filepath, doc)
    }
    class AppsGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    class InfraGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    class LibsGenerator {
        prompt(prompts) Promise
        route()
        confirm(tpl) Promise
    }
    class MasterGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class GeneratorGenerator {
        String[] _templates
        initializing()
        prompting() Promise
        configuring()
        composing()
        routing()
        confirming() Promise
        writing()
        install()
    }
    class ClassGeneratorGenerator {
        String _typeJs
        String _indexJs
        String[] _templates
        initializing()
        prompting() Promise
        configuring()
        routing()
        confirming() Promise
        _updateTypeJs()
        _updateIndexJs()
        writing()
    }
    class FrameworkGeneratorGenerator {
        String _frameworkJs
        initializing()
        prompting() Promise
        configuring()
        routing()
        confirming() Promise
        writing()
    }
    class YoGeneratorGenerator {
        String _yoBuildBazel
        initializing()
        prompting() Promise
        routing()
        confirming() Promise
        writing()
    }
    class CaviarGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class HelloCaviarGenerator {
        String _templatesPattern
        initializing()
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
    class ShuviGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class JsShuviGenerator {
        String _templatesPattern
        initializing()
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
    class CicdGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class WebappCicdGenerator {
        String[] _files
        String[] _templates
        prompting() Promise
        routing()
        confirming() Promise
        _getUploadStaticToS3() String
        _createPipelineRunSpecYamlNode(doc, options) Node
        _createPresubmitYamlNode(doc) Node
        _createPostsubmitYamlNode(doc) Node
        writing()
    }
    class LibGenerator {
        initializing()
        prompting() Promise
        composing()
    }
    class JsLibGenerator {
        String _templatesPattern
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
    class TsLibGenerator {
        String[] _templates
        prompting() Promise
        routing()
        confirming() Promise
        writing()
        install()
    }
    link EventEmitter "https://nodejs.org/api/events.html#events_events" "Click to see spec"
    link MemFsEditor "https://github.com/SBoudrias/mem-fs-editor" "Click to see spec"
    link YeomanGenerator "https://github.com/yeoman/generator" "Click to see spec"
`}/>
