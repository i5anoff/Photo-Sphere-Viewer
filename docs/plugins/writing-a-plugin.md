# Writing a plugin

[[toc]]

## Syntax

The recommended way to create your own plugin is as an ES6 class extending `AbstractPlugin` provided by `photo-sphere-viewer` core package.

The plugin class **must** take a `PSV.Viewer` object as first parameter and pass it to the `super` constructor. It also have to implement `init` and `destroy` methods wich are used to bootstrap and cleanup the plugin.

In you plugin you have access to `this.psv` which is the instance of the viewer, check the [API Documentation](https://photo-sphere-viewer.js.org/api/PSV.Viewer.html) for more information.

Your plugin is also an [`EventEmitter`](https://github.com/mistic100/uEvent) with `on`, `off` and `trigger` methods.

```js
import { AbstractPlugin } from 'photo-sphere-viewer';

export default class PhotoSphereViewerCustomPlugin extends AbstractPlugin {
  
  constructor(psv) {
    super(psv);
  }
  
  init() {
    super.init();
    
    // do your initialisation logic here
  }
  
  destroy() {
    // do you cleanup logic here
    
    super.destroy();
  }
  
}
```

Beside this main class, you can use any number of ES modules to split your code.


## Packaging

The simplest way to package your plugin is by using [rollup.js](https://rollupjs.org) and [Babel](https://babeljs.io) with the following configuration:

```js
// rollup.config.js

export default {
  input   : 'index.js',
  output  : {
    file     : 'browser.js',
    name     : 'PhotoSphereViewerCustomPlugin',
    format   : 'umd',
    sourcemap: true,
    globals  : {
      'three'              : 'THREE',
      'uevent'             : 'uEvent',
      'photo-sphere-viewer': 'PhotoSphereViewer',
    },
  },
  external: [
    'three',
    'uevent',
    'photo-sphere-viewer',
  ],
  plugins : [
    require('rollup-plugin-babel')({
      exclude: 'node_modules/**',
    }),
  ],
};
```

```json
// .babelrc

{
 "presets": [
   ["@babel/env", { "loose": true } ]
 ]
}
```

### Stylesheets

If your plugin requires custom CSS, import the stylesheet directly in your main Javascript file and add this rollup plugin to your configuration (here I use a SASS loader):

```js
require('rollup-plugin-postcss')({
  extract  : true,
  sourceMap: true,
  plugins  : [
    require('@csstools/postcss-sass')({}),
    require('autoprefixer')({}),
  ],
})
```


## Naming and publishing

If you intend to publish your plugin on npmjs.org please respect the folowing naming:

- class name and export name (in rollup config file) : `PhotoSphereViewer[[Name]]Plugin`
- NPM package name : `photo-sphere-viewer-[[name]]-plugin`

Your `package.json` must be properly configured to allow application bundlers to get the right file, and `photo-sphere-viewer` must be declared as peer dependency.

```json
{
  "name": "photo-sphere-viewer-custom-plugin",
  "version": "1.0.0",
  "main": "browser.js",
  "module": "index.js",
  "peerDependencies": {
    "photo-sphere-viewer": "^4.0.0"
  }
}
```
