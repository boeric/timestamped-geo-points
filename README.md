# timestamped-geo-points
Expands an array of geo vertices to timestamped geo points. Previously captured verticies (input file) indicate geo locations where a geo path is changing direction. For example, a vehicle's path along a road may be captured with a set of verticies, each of which indicate where the vechicle's path is changing direction. The purpose with the `expandTime` module is to expand the array of verticies to an array of geo points which are spaced evenly in the time domain, and interpolated properly between the input verticies. Each vertex can have its own speed, which will be maintained for all points generated between the vertex and the following vertex. One geo point is generated each **second** using the current "vertex speed". The vertex speed is a property of the vertex geo point (please see an example of a generator that captures both speed and the geo point [here](https://github.com/boeric/VertexGenerator)). If such property is unavailable in the input GeoJson data, this software defaults to 35Mph.

### Installation, etc.
1. Clone the repo: `git clone https://github.com/boeric/timestamped-geo-points.git`
2. Install the dependencies: `npm install`
3. Run `expandTime`: `node expandTime inputGeo.json outputgeo.json`

### Input file
The input file must be a valid GeoJson file. In the example below, the first feature of a GeoJson feature collection is shown. This point, here called a vertex, has an optional `speed` value among its properties. If such value is present, the `expandTime.js` module will take it account and properly space the output points

```
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "type": "Point",
        "coordinates": [
          -121.9563121795567,
          37.35941523522688
        ]
      },
      "properties": {
        "id": "1493695500267",
        "speed": 35
      }
    },
    ...
```

### Output file
The output file is a GeoJson file of a Feature collection with the points generated while processing the input GeoJson file. It can be rendered in any tool that accepts GeoJson inputs

### Command line
```
node expandTime input-file output-file
for example: 
   node expandTime input.json output.json
   node expandTime input.json
```
If the output file is missing, the output GeoJson will be sent to standard output.

### Test files
The repo contains one input test file, `octagonGeo.json`. If this is used as input to `expandTime.js`, the output should be identical to `outputGeo.json`, also included in the repo. Please see the included `png` screenshots of the input and output files.

### Algorithm

![screenshot](https://github.com/boeric/timestamped-geo-points/blob/master/screenshots/algorithm.png)


