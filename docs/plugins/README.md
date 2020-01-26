# Introduction to plugins

Plugins are used to add new functionalities to Photo Sphere Viewer. They can access all internal APIs of the viewer as well as the THREE.js renderer to make the viewer even more awesome.

## Using a plugin

All plugins consists of a JavaScript class which must be instanciated with an existing viewer as first parameter. Some plugins will also take configuration object as second parameter.

```js
const viewer = new PhotoSphereViewer.Viewer({ ... });

const examplePlugin = new PhotoSphereViewer.ExamplePlugin(viewer);
```

Once created, the lifecycle of the plugin is tied to its viewer : it will initialized when the viewer is ready and destroyed with the viewer if needed.
