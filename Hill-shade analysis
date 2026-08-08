// =====================================
// ROI
// =====================================
var roiFc = ee.FeatureCollection('projects/peaceful-crane-486406-v9/assets/Export_Output');
var roi = roiFc.geometry();

Map.centerObject(roi, 11);
Map.addLayer(roiFc.style({color:'000000', width:2, fillColor:'00000000'}), {}, 'ROI');

// =====================================
// Best DEM for USA: USGS 3DEP 10m
// =====================================
var dem = ee.Image('USGS/3DEP/10m')
  .select('elevation')
  .clip(roi)
  .rename('Elevation_m');

// Hillshade (GEE built-in)
var hillshade = ee.Terrain.hillshade(dem).rename('Hillshade');

// Display
Map.addLayer(dem, {min: 800, max: 1100}, 'Elevation (m)');
Map.addLayer(hillshade, {min: 0, max: 255}, 'Hillshade');

// Optional: pretty shaded relief (hillshade * elevation stretch)
var demVis = dem.visualize({min: 800, max: 1100});
var hsVis = hillshade.visualize({min: 0, max: 255});
Map.addLayer(hsVis, {}, 'Hillshade (visual)');

// Export Elevation
Export.image.toDrive({
  image: dem,
  description: 'Elevation_3DEP10m',
  folder: 'GEE_Exports',
  region: roi,
  scale: 10,
  crs: 'EPSG:32614',     // UTM 14N for Lubbock area
  maxPixels: 1e13
});

// Export Hillshade
Export.image.toDrive({
  image: hillshade,
  description: 'Hillshade_3DEP10m',
  folder: 'GEE_Exports',
  region: roi,
  scale: 10,
  crs: 'EPSG:32614',
  maxPixels: 1e13
});
