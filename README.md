# SVGPathCommander
A modern set of ES6/ES7 JavaScript tools for manipulating *SVGPathElement* description attribute. This library was developed to try and solve over-optimized `arcTo` segment strings and provide a solid solution to parse, convert and reverse *SVGPathElement* draw direction.

This library is made possible thanks to [Raphael.js](https://dmitrybaranovskiy.github.io/raphael/), [SvgPath](https://github.com/fontello/svgpath), [Paper.js](https://github.com/paperjs/paper.js/) and is used by [KUTE.js](https://github.com/thednp/kute.js) for [SVG path morphing](https://thednp.github.io/kute.js/svgCubicMorph.html).

[![NPM Version](https://img.shields.io/npm/v/svg-path-commander.svg?style=flat-square)](https://www.npmjs.com/package/svg-path-commander)
[![NPM Downloads](https://img.shields.io/npm/dm/svg-path-commander.svg?style=flat-square)](http://npm-stat.com/charts.html?svg-path-commander)
[![jsDeliver](https://data.jsdelivr.com/v1/package/npm/svg-path-commander/badge)](https://www.jsdelivr.com/package/npm/svg-path-commander)

# Install

```
npm install svg-path-commander
```

# CDN

Find SVGPathCommander on [jsDelivr](https://www.jsdelivr.com/package/npm/svg-path-commander).

# Usage

On a regular basis, you can import, initialize and access methods, or return the values right away.

```js
// import the constructor
import SVGPathCommander from 'svg-path-commander'

let pathString = 'M0 0l50 0l50 50z';

// initializing
let mySVGPathCommanderInit = new SVGPathCommander(pathString);
/* returns => {
  segments: [ ['M',0,0], ['l',50,0], ['l',50,50], ['z'] ]
}
*/
```

# Methods

The SVGPathCommander construct comes with various methods you can call one-by-one or 
```js
// reuse same init object to call different methods
// for instance convert to ABSOLUTE and return the initialization object
mySVGPathCommanderInit.toAbsolute()

// or convert to RELATIVE and return the string path directly
mySVGPathCommanderInit.toRelative().toString()

// reverse and return the string path
mySVGPathCommanderInit.reverse().toString()

// ONLY reverse subpaths and return the string path
// if the shape has no sub-path, this call will produce no effect
mySVGPathCommanderInit.reverse(1).toString()

// converts to both absolute and relative then return the shorter segment string
mySVGPathCommanderInit.optimize().toString()

// or return directly what you need
let mySVGAbsolutePath = new SVGPathCommander(pathString)
                        .reverse(1)
                        .optimize()
                        .toString()
```

# Advanced Usage

In some cases, you can also import only the tool you need, without importing the entire library.

```js
import pathToAbsolute from 'svg-path-commander/src/convert/pathToAbsolute.js'
import pathToString from 'svg-path-commander/src/convert/pathToString.js'

let mySVGAbsolutePath = pathToString(pathToAbsolute(pathString))
```

# Tools
When using the library as a package, type in "SVGPathCommander." in your browser console and have a look, there are a wide range of tools to play with. Here are some notable utilities:

* `SVGPathCommander.parsePathString(pathString)` - returns a *pathArray* and is used by/for most of SVGPathCommander conversion tools
* `SVGPathCommander.pathToAbsolute(pathString|pathArray)` - returns a new *pathArray* having all path commands as **absolute** coordinates
* `SVGPathCommander.pathToRelative(pathString|pathArray)` - returns a new *pathArray* having all path commands as **relative** coordinates
* `SVGPathCommander.pathToCurve(pathString|pathArray)` - returns a new *pathArray* having all path commands converted to `cubicBezierTo` (`C`) path commands
* `SVGPathCommander.clonePath(pathArray)` - returns a **deep clone** of a *pathArray*, which is the result of any of the above functions
* `SVGPathCommander.roundPath(pathArray)` - returns a new *pathArray* with all path command values rounded to 3 decimals by default
* `SVGPathCommander.reversePath(pathString|pathArray)` - returns a new *pathArray* with all path commands having absolute values and in reverse order, but only for a single M->Z shape, for *SVGPathElement* items with sub-path, you need to use `pathToAbsolute` -> `pathToString` -> `splitPath` -> `reversePath` for each subpath
* `SVGPathCommander.optimizePath(pathArray)` - returns a new *pathArray* with all segments that have the shortest strings from either absolute or relative `pathArray` segments
* `SVGPathCommander.getDrawDirection(pathCurve)` - returns **TRUE** if a shape draw direction is **clockwise**, it should not be used for shapes with sub-paths, but each sub-path individually
* `SVGPathCommander.splitPath(pathString)` - returns an *Array* of sub-path strings

# Some Technical Considerations
* the `optimize()` method will only return the shortest segment string of the *pathArray*, it will not simplify/merge the path commands; to do that you can use [SVGO](https://github.com/svg/svgo) and its `convertPathData` plugin;
* all tools processing path segments will always round float values to 3 decimals, remember: only float numbers; EG: 0.5666 => 0.566, 0.50 => 0.5, 5 => 5; you can change the default option with `SVGPathCommander.options.decimals = 2` or remove the value rounding all together with `SVGPathCommander.options.round = 0`.


# Special Thanks

* Dmitry Baranovskiy for his [Raphael.js](https://dmitrybaranovskiy.github.io/raphael/)
* Vitaly Puzrin & Alex Kocharin for their [SvgPath](https://github.com/fontello/svgpath)
* Jürg Lehni & Jonathan Puckey for their [Paper.js](https://github.com/paperjs/paper.js/)
* Andrew Willems for his [awesome guide](https://stackoverflow.com/users/5218951/andrew-willems)
* Mike 'Pomax' Kamermans for his awesome [svg-path-reverse](https://github.com/Pomax/svg-path-reverse) and [bezierjs](https://github.com/Pomax/bezierjs)

# License
SVGPathCommander is released under [MIT Licence](https://github.com/thednp/svg-path-commander/blob/master/LICENSE).