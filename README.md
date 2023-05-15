# Air_Quality_Index
### Firstly add your area of Interset in Earth engine console, then folow the below codes
var collection = ee.ImageCollection('COPERNICUS/S5P/OFFL/L3_NO2')
  .select('tropospheric_NO2_column_number_density')
  .filterDate('2023-01-01', '2023-01-30')
  .filterBounds(roi).mean().clip(roi);

var band_viz = {
  min: 0,
  max: 0.0003,
  palette: ['black', 'blue', 'purple', 'cyan', 'green', 'yellow', 'red']
};

Map.addLayer(collection, band_viz,'N02');

Map.centerObject(roi, 10);

var panel = ui.Panel({
  style: {
    position: 'bottom-right',
    padding: '5px;'
  }
})

var title = ui.Label({
  value: 'N02',
  style: {
    fontSize: '14px',
    fontWeight: 'bold',
    margin: '0px;'
  }
})

panel.add(title)

var color = ['green','cyan','yellow','red']

var lc_class = ['Very Low', 'Low', 'High', 'Very High']

var list_legend = function(color, description) {
  
  var BK = ui.Label({
    style: {
      backgroundColor: color,
      padding: '10px',
      margin: '4px'
    }
  })
  
  var sum = ui.Label({
    value: description,
    style: {
      margin: '5px'
    }
  })
  
  return ui.Panel({
    widgets: [BK, sum],
    layout: ui.Panel.Layout.Flow('horizontal')
  })
}

for(var a = 0; a < 4; a++){
  panel.add(list_legend(color[a], lc_class[a]))
}

Map.add(panel)

Export.image.toDrive({
  image:collection,
  description:'N02',
  region:roi,
  scale:30,
  crs:'EPSG:32646'
});
