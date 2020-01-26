# AutorotateKeypointsPlugin ([API](https://photo-sphere-viewer.js.org/api/PSV.plugins.AutorotateKeypointsPlugin.html))

> Replaces the standard autorotate animation by a smooth transition between multiple points.

This plugin is available in the core `photo-sphere-viewer` package at `plugins/autorotate-keypoints-plugin.js`.


## Usage

The plugin is configured with `keypoints` which can be either a position object (`longitude` + `latitude`) or the identifier of an existing [marker](../guide/markers).

It is also possible to configure each keypoint with a pause time and a tooltip.

```js
const keypointsPlugin = new PhotoSphereViewer.AutorotateKeypointsPlugin(viewer);

keypointsPlugin.setKeypoints([
  'existing-marker-id',
  
  { longitude: Math.PI / 2, latitude: 0 },
  
  {
    position: { longitude: Math.PI, latitude: Math.PI / 6 },
    pause   : 5000,
    tooltip : 'This is interesting',
  },
  
  {
    markerId: 'another-marker', // will use the marker tooltip if any
    pause   : 2500,
  },
]);
```

The plugins reacts to the standard `autorotateDelay` and `autorotateSpeed` options and can be started with `startAutorotate` or the button in the navbar.


## Demo

TODO


## Configuration

The second parameter of the plugin accepts the following options:

#### `startFromClosest`
- type: `boolean`
- default: `true`

Start from the closest keypoint instead of the first keypoint of the array.

#### `keypoints`
- type: `Keypoints[]`

Initial keypoints, does the same thing as calling `setKeypoints` just after initialisation.
