---
id: arch-compiler
title: Compiler
sidebar_label: Compiler
---

## Background

1. mini-program framework based on Vue.js
2. we use Vue SFC to define pages and components
3. under mini-program runtime, we need to disable many of Vue's native commands/directives and behaviors, and only allow the user to write templates to generate the corresponding html

For these reasons, we need a custom compiler to compile the code in Vue SFC into code that can be run by the mini-program framework.

The Vue SFC file format is as follows:

```vue
<template>
  <div :class="test">
    {{ count }}  
  </div>
</template>

<script>
  export default {
    data() {
      return {
        count: 1            
      }
    }  
  }
</script>

<style scoped>
  .test {
    color: red;
  }      
</style>
```

## Design

The compiler contains three parts: `compiler-core`, `compiler-dom`, and `compiler-sfc`. Among them, `compiler-dom` is used to handle dom events, and `compiler-sfc` is used to parse the SFC file and compile the parsed code block into js code respectively. The dependencies between them are as follows.

![compiler-dependence](https://static.devfdg.net/static/mono-static/docs-ui/img/compiler/compiler-dependence.png)

### compiler-core

`compiler-core` consists of three main modules, `parser`, `transformer`, and `generator`, which do the following:

1. `parser` parse **template** in Vue SFC to AST
2. `transformer` transform AST for `render/worker` runtime
3. `generator` generate `component.render` function by transformed AST

![core](https://static.devfdg.net/static/mono-static/docs-ui/img/compiler/core.png)

### compiler-dom

`compiler-dom` mainly contains the transform method (`DOMDirectiveTransforms`) of the dom directive, which allows the template to support dom directives such as `v-on`, `v-html` etc.

### compiler-sfc

`compiler-sfc` consists of four main modules: `parser`, `template-compiler`, `script-compiler`, and `style-compiler`, which do the following:

1. `parser` parse the code blocks in the Vue SFC file into three blocks, `TemplateBlock`, `ScriptBlock`, and `StyleBlock`.
2. `template-compiler` compile the code in `TemplateBlock` into a **render function** via `compiler-dom`.
3. `script-compiler` analyze the code in `ScriptBlock` and finally return the component **js bundle** for `render/worker` runtime
4. `style-compiler` generate a **css bundle** by the code in `StyleBlock` with stylePreprocessor(sass/less/stylus) and postcss.

![sfc](https://static.devfdg.net/static/mono-static/docs-ui/img/compiler/sfc.png)

## Code Bundle

we use webpack to bundle the code that needed for the mini-program framework. 

### vue-loader

`vue-loader` can read the `*.vue` file, and it use `compiler-sfc` to generate the **render function**, **js bundle** and **css bundle**, then combine the three parts into one js file in webpack bundle process.
