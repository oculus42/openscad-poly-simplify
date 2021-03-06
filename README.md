# osPoly - openscad-poly-tools
A library to manipulate OpenSCAD Polyhedrons

[![npm](https://img.shields.io/npm/v/openscad-poly-tools.svg)](https://www.npmjs.com/package/openscad-poly-tools) 
[![Maintainability](https://api.codeclimate.com/v1/badges/3c18a09cf93fa7a24b88/maintainability)](https://codeclimate.com/github/oculus42/openscad-poly-tools/maintainability)

This library is still in development (pre 1.0.0) and may change *significantly* between minor versions.
Once it reaches 1.0.0 all breaking changes will use a major revision number, as dictated by semver.

## HTML Convertor
An update to the original STL to OpenSCAD convertor, which eliminate the extra vertices, is included as `convert.html`. 
It is also [available online at JSFiddle](http://jsfiddle.net/_sir/yzvGD/595/). 

As of 0.10.0, the library supports converting STL to OpenSCAD on NodeJS.

## Functions

### Convert Functions

#### load(filename)
Load an STL as an object for manipulation, using the [stl](https://www.npmjs.com/package/stl) package.

Returns a Promise.

```javascript
myModelPromise = osPoly.load('./test.stl');
  
// Promise chaining example
osPoly.load('./example.stl')
  .then(model => osPoly.center(model, 'x'));
```

#### format(model, [moduleIndex])
Convert an object into a string equivalent of an OpenSCAD document as a step before saving.
Supports an optional index to allow multiple objects to save into a file.

```javascript
osPoly.format(myModel);
```

#### save(filename, model)
Save the object as an OpenSCAD file. Currently supports only a single object.

Returns a Promise.

```javascript
savePromise = osPoly.save('./example.scad', myModel);
```

#### process(stlFile, scadFile)
Import an STL file, simplify it, and export an OpenSCAD document.
This provides my most common workflow as a single command.

```javascript
osPoly.process('./example.stl', './example.scad');
  
// Chaining example
osPoly.process('./example.stl', './example.scad')
  .then(() => console.log('Success'), e => console.log(e));
```

### Simplify Functions

#### reportDuplicatePercentage(model)
Give the percentage of duplicates in the array of points. No changes are made.

```javascript
osPoly.reportDuplicatePercentage(myModel);
```

#### simplify(model)
Eliminate duplicate points in the model's `points` array and re-map the indices in the `faces` to match.

```javascript
osPoly.simplify(myModel);
```

### Translate Functions

### center(model, axis)
Move the center (average of the highest and lowest points) of a model to the origin of the specified axis.

```javascript
osPoly.center(myModel, 'x')
```

#### moveToOrigin(model, axis, [moveTop])
Move the lowest or highest point on an axis to the origin.

```javascript
osPoly.moveToOrigin(myModel, 'z')
```

#### translate(model, vector)
Shift the model along one or more axes.

```javascript
osPoly.translate(myModel, { x: 10, y: -2, z: 0.25 });
```

### Edit

#### filterForMatch(model, predicate)
Get only the faces of the model which have points matching the predicate.

**NOTE:** This is not the same as a Boolean operation in OpenSCAD. Any faces that have points eliminated by the predicate will be removed, not modified.

```javascript
osPoly.filterForMatch(myModel, (point) => point.x > 0);
``` 
  
#### removeDeadPoints(model)
Eliminate any dead points and correct the indices of the faces to match.

```javascript
osPoly.removeDeadPoints(myModel);
```

## Notes

### Conventions

Nearly all functions take a model, which is an object with arrays of points and faces.

```javascript
myModel = {
  points: [],
  faces: [],
};
```

Where an axis is specified, the library should accept the string `'x'`, `'y'`, or `'z'`; or their
index `0`, `1`, or `2`, respectively.

The original objects passed in are not modified.
In some cases, the returned points and faces may reference the same original arrays.

### Compatibility

This library is developed on Node 9.x, and is tested against 8.x.
It uses functionality not available on previous releases.
If you would like to use it with an earlier version of NodeJS, please open an issue on GitHub.

### Motivation
After importing an STL into OpenSCAD with [Riham's STL to OpenSCAD Convertor](http://jsfiddle.net/Riham/yzvGD/)
on JSFiddle and [Thingiverse](https://www.thingiverse.com/thing:62666)), I discovered a significant duplication of points.
I started working on a way to correct this. Along the way, this library was created.

### Goals
* ~~Perform simple, brute-force point de-duplication on polyhedrons.~~
* ~~Move objects~~
* ~~Cut parts of an object~~
* ~~Provide a consistent interface for all functions~~
* ~~Incorporate an update to Riham's original importer~~
* ~~Provide a Node-friendly version of the import functionality~~
* Identify faces sharing points in the same plane and merge them
* Support multiple object loading in Node version
* Update the HTML convertor visuals
* Add tests?
* Provide ES5 version?
* Continue to provide "web" version?
* Support earlier Node versionss?
