# osPoly - openscad-poly-tools
A library to manipulate OpenSCAD Polyhedrons

## Motivation
After importing an STL into OpenSCAD with [http://jsfiddle.net/Riham/yzvGD/](http://jsfiddle.net/Riham/yzvGD/) I realized there was a significant duplication of points.

### JSFiddle Update
I also [forked the JSFiddle](http://jsfiddle.net/_sir/yzvGD/595/) with a version that prevents the extra vertices.

## Features

### osPoly.simplify({points, faces});

```javascript
osPoly.simplify({
  points: myPolyPoints,
  faces: myPolyFaces
});

```

Removes duplicate points in the `points` array and remaps the indexes in `faces`.

### osPoly.translate(points, offset);

```javascript
osPoly.translate(myPoints, { x: 5, y: 0, z: -1});
```

Recenter the object 

`axisIndex` matches the index of the `[x, y, z]` coordinates (i.e. `0`, `1`, `2`).  
`moveTop` is a Boolean that indicates whether the smallest (`false`) or largest (`true`) value will move to origin.


### osPoly.moveToAxis(points, axisIndex, moveTop);

```javascript
osPoly.moveToAxis(myPoints, 1);
```

Intended to *drop* an object to the origin of an axis.

`axisIndex` matches the index of the `[x, y, z]` coordinates (i.e. `0`, `1`, `2`).  
`moveTop` is a Boolean that indicates whether the smallest (`false`) or largest (`true`) value will move to origin.


## Goals
* ~~Perform simple, brute-force point de-duplication on polyhedrons.~~
* ~~Move objects~~
* ~~Cut parts of an object~~
* Incorporate the importer from https://www.thingiverse.com/thing:62666
* Provide a consistent interface for all functions
* Identify faces sharing points in the same plane and merge them
