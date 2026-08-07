// ===============================
// 1. ROI
// ===============================
var roi = ee.FeatureCollection("projects/peaceful-crane-486406-v9/assets/Export_Output");

// ===============================
// 2. Load 10m DEM: FABDEM
// ===============================
var dem = ee.Image("projects/sat-io/open-datasets/FABDEM/FABDEM_V1-2")
  .select("b1")
  .clip(roi)
  .rename("DEM");

// ===============================
// 3. Smooth DEM
// ===============================
var demSmooth = dem.focal_mean({
  radius: 20,
  units: "meters"
});

// ===============================
// 4. Calculate TRI
// TRI = mean absolute difference between center cell and neighbors
// ===============================
var kernel = ee.Kernel.square({
  radius: 1,
  units: "pixels",
  normalize: false
});

var neighborhood = demSmooth.neighborhoodToBands(kernel);

var tri = neighborhood
  .subtract(demSmooth)
  .abs()
  .reduce(ee.Reducer.mean())
  .rename("TRI_10m");

// ===============================
// 5. Visualization
// ===============================
var triVis = {
  min: 0,
  max: 10,
  palette: ["006400", "ffff00", "ff0000"]
};

Map.centerObject(roi, 10);
Map.addLayer(tri, triVis, "TRI 10m");

// ===============================
// 6. Export GeoTIFF
// ===============================
Export.image.toDrive({
  image: tri,
  description: "TRI_10m",
  folder: "GEE_Outputs",
  fileNamePrefix: "TRI_10m",
  region: roi.geometry(),
  scale: 10,
  crs: "EPSG:4326",
  maxPixels: 1e13
});
