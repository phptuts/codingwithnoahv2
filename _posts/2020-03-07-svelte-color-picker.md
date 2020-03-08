---
layout: post
title: Svelte Color Picker Part 1
date: 2020-03-07 16:32 -0800
tags: ['svelte color picker', 'svelte', 'svelte material ui', 'blueprint css']
---

## Part 1
<iframe width="560" height="315" src="https://www.youtube.com/embed/5l42na3hGLE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Part 2

## Part 3

## Code 

[Code](https://github.com/phptuts/yt-svelte-color-picker-1)

[Code](https://github.com/phptuts/yt-svelte-color-picker-2)

## Demo

[Demo Part 1](https://phptuts.github.io/yt-svelte-color-picker-1/)

![Tutorial Demo Image](/assets/2020-03-07-svelte-color-picker/tutorial-demo.png)

[Finished Demo](https://phptuts.github.io/yt-svelte-color-picker-2/)

![Final Image](/assets/2020-03-07-svelte-color-picker/final-demo.png)


## NPM Install

```bash
npm install --saved-dev blueprint-css sass rollup-plugin-postcss @smui/button @smui/data-table @smui/form-field @smui/slider @smui/switch @smui/textfield gh-pages

```


## Rollup Config

```javascript
import svelte from 'rollup-plugin-svelte';
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';
import livereload from 'rollup-plugin-livereload';
import { terser } from 'rollup-plugin-terser';
import postcss from 'rollup-plugin-postcss';
import ghpages from 'gh-pages';

const production = !process.env.ROLLUP_WATCH;

export default {
  input: 'src/main.js',
  output: {
    sourcemap: true,
    format: 'iife',
    name: 'app',
    file: 'public/build/bundle.js'
  },
  plugins: [
    svelte({
      // enable run-time checks when not in production
      dev: !production,
      // we'll extract any component CSS out into
      // a separate file - better for performance
      emitCss: true
    }),

    postcss({
      extract: true,
      minimize: true,
      use: [
        [
          'sass',
          {
            includePaths: ['./theme', './node_modules']
          }
        ]
      ]
    }),
    // If you have external dependencies installed from
    // npm, you'll most likely need these plugins. In
    // some cases you'll need additional configuration -
    // consult the documentation for details:
    // https://github.com/rollup/plugins/tree/master/packages/commonjs
    resolve({
      browser: true,
      dedupe: ['svelte']
    }),
    commonjs(),

    // In dev mode, call `npm run start` once
    // the bundle has been generated
    !production && serve(),

    // Watch the `public` directory and refresh the
    // browser on changes when not in production
    !production && livereload('public'),

    // If we're building for production (npm run build
    // instead of npm run dev), minify
    production &&
      terser() &&
      ghpages.publish('public', () => {
        console.log('uploaded');
      })
  ],
  watch: {
    clearScreen: false
  }
};

function serve() {
  let started = false;

  return {
    writeBundle() {
      if (!started) {
        started = true;

        require('child_process').spawn('npm', ['run', 'start', '--', '--dev'], {
          stdio: ['ignore', 'inherit', 'inherit'],
          shell: true
        });
      }
    }
  };
}
```
## Color Table Svelte Imports Part 3

```javavascript
  import DataTable, { Head, Body, Row, Cell } from "@smui/data-table";
  import { onMount } from "svelte";
  import Textfield from "@smui/textfield";
  import Switch from "@smui/switch";
  import FormField from "@smui/form-field";
```

## Revelant Links

- [Svelte Material UI](https://sveltematerialui.com/)
- [Svelte Material UI Github](https://github.com/hperrin/svelte-material-ui)
- [Svelte Material UI Rollup Example](https://github.com/hperrin/smui-example-rollup)
- [Material Design](https://material.io/design/)
- [Material CSS Library](https://github.com/material-components/material-components-web)
- [Blueprint css](https://blueprintcss.dev/)
- [Emit Css Docs](https://github.com/sveltejs/rollup-plugin-svelte#extracting-css)
- [Slider Package](https://github.com/hperrin/svelte-material-ui/tree/master/packages/slider)
- [Switch Package](https://github.com/hperrin/svelte-material-ui/tree/master/packages/switch)
- [Text Field](https://github.com/hperrin/svelte-material-ui/tree/master/packages/textfield)
- [Datatable](https://github.com/hperrin/svelte-material-ui/tree/master/packages/datatable)

## Revelant Svelte Tutorials

- [Writable Stores](https://svelte.dev/examples#writable-stores)
- [Prop Binding](https://svelte.dev/examples#component-bindings)
- [Svelte Styles Article](https://css-tricks.com/what-i-like-about-writing-styles-with-svelte/)
- [Reactive Variables](https://svelte.dev/examples#reactive-declarations)
