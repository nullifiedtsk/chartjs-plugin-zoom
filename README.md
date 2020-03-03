# chartjs-plugin-zoom

A zoom and pan plugin for Chart.js. Currently requires Chart.js >= 2.6.0

Panning can be done via the mouse or with a finger.
Zooming is done via the mouse wheel or via a pinch gesture. [Hammer.js](https://hammerjs.github.io/) is used for gesture recognition.

[Live Codepen Demo](https://codepen.io/jledentu/pen/NWWZryv)

## Installation

Run `npm install --save chartjs-plugin-zoom` to install with `npm`.

If including via a `<script>` tag, make sure to include `Hammer.js` as well:

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@2.9.1"></script>
<script src="https://cdn.jsdelivr.net/npm/hammerjs@2.0.8"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-zoom@0.7.4"></script>
```

## Configuration

To configure the zoom and pan plugin, you can simply add new config options to your chart config.

```javascript
plugins: {
	zoom: {
		// Container for pan options
		pan: {
			// Boolean to enable panning
			enabled: true,

			// Panning directions. Remove the appropriate direction to disable
			// Eg. 'y' would only allow panning in the y direction
			// A function that is called as the user is panning and returns the
			// available directions can also be used:
			//   mode: function({ chart }) {
			//     return 'xy';
			//   },
			mode: 'xy',

			rangeMin: {
				// Format of min pan range depends on scale type
				x: null,
				y: null
			},
			rangeMax: {
				// Format of max pan range depends on scale type
				x: null,
				y: null
			},

			// Function called while the user is panning
			onPan: function({chart}) { console.log(`I'm panning!!!`); },
			// Function called once panning is completed
			onPanComplete: function({chart}) { console.log(`I was panned!!!`); }
		},

		// Container for zoom options
		zoom: {
			// Boolean to enable zooming
			enabled: true,

			// Enable drag-to-zoom behavior
			drag: true,

			// Drag-to-zoom effect can be customized
			// drag: {
			// 	 borderColor: 'rgba(225,225,225,0.3)'
			// 	 borderWidth: 5,
			// 	 backgroundColor: 'rgb(225,225,225)',
			// 	 animationDuration: 0
			// },

			// Zooming directions. Remove the appropriate direction to disable
			// Eg. 'y' would only allow zooming in the y direction
			// A function that is called as the user is zooming and returns the
			// available directions can also be used:
			//   mode: function({ chart }) {
			//     return 'xy';
			//   },
			mode: 'xy',

			rangeMin: {
				// Format of min zoom range depends on scale type
				x: null,
				y: null
			},
			rangeMax: {
				// Format of max zoom range depends on scale type
				x: null,
				y: null
			},

			// Speed of zoom via mouse wheel
			// (percentage of zoom on a wheel event)
			speed: 0.1,

			// Function called while the user is zooming
			onZoom: function({chart}) { console.log(`I'm zooming!!!`); },
			// Function called once zooming is completed
			onZoomComplete: function({chart}) { console.log(`I was zoomed!!!`); },
			
			/**
			 * @typedef {Object} MinMaxValue
			 * @property {number} min - Min value
			 * @property {number} max - Max value
			 */
			/**
			 * @typedef {Object} PendingScaleChanges - Represents min/max value updates for ticks and time options.
			 * @property {MinMaxValue} ticks - new options.ticks values
			 * @property {MinMaxValue} time - new options.time values (may be undefined)
			 */
			/**
			 * This function may be used to filter unnessecary zoom events. Returns true to allow zooming, false to prevent.
			 * (default ticks.min and ticks.max will also limit zoom range)
			 * @param {Chart} chart - Chart instance.
			 * @param {Scale} scale - Scale/Axis being zoomed (x/y, method will be called for each axis)
			 * @param {ChartOptions} options - Original chart options before any zoom values applied.
			 * @param {PendingScaleChanges} changes - Changes that will be applied to chart options if method returns true.
			allowZoom: function(chart, scale, options, changes) {
			  if (scale.id === 'x-axis-0') {
				const millisecondsInSecond = 1000;
				const secondsInViewBefore = (beforeZoom.ticks.max - beforeZoom.ticks.min) / millisecondsInSecond;
				const secondsInViewAfter = (changes.ticks.max - changes.ticks.min) / millisecondsInSecond;
				
				// Allow to zoom until there will be 60 seconds in view and allow to zoom out.
				// (default ticks.min and ticks.max will also limit zoom range)
				if (secondsInViewAfter < 60 && secondsInViewAfter <= secondsInViewBefore) {
					return false;
				}
			  }
			  return true;
			}
		}
	}
}
```

## API

### chart.resetZoom()

Programmatically resets the zoom to the default state. See [samples/zoom-time.html](samples/zoom-time.html) for an example.

## Installation

To download a zip, go to the chartjs-plugin-zoom.js on Github

To install via npm / bower:

```bash
npm install chartjs-plugin-zoom --save
```

Prior to v0.4.0, this plugin was known as 'Chart.Zoom.js'. Old versions are still available on npm under that name.

## Documentation

You can find documentation for Chart.js at [www.chartjs.org/docs](https://www.chartjs.org/docs).

Examples for this plugin are available in the [samples folder](samples).

## Contributing

Before submitting an issue or a pull request to the project, please take a moment to look over the [contributing guidelines](CONTRIBUTING.md) first.

## License

chartjs-plugin-zoom.js is available under the [MIT license](https://opensource.org/licenses/MIT).
