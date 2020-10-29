# Cordoba-Wildfires


web map creado en qgis.

Imagenes obtenidas y editadas en Google Earth Engine

Productos utilizados

MODIS/006/MOD14A1 Fecha de inicio: 2020-07-01,Fecha Finalizacion: 2020-10-25

COPERNICUS/S2_SR
Pre Incendios: Fecha de inicio: 2020-05-01,Fecha Finalizacion: 2020-07-01 Post Incendios: Fecha de inicio: 2020-09-01,Fecha Finalizacion: 2020-10-25

codigo Google Earth Engine

//AOI Provincia de Cordoba var cordoba = ee.FeatureCollection('users/mauropicatto/provincia') .filter(ee.Filter.eq('gid', 13))

/**

    Function to mask clouds using the Sentinel-2 QA band
    @param {ee.Image} image Sentinel-2 image
    @return {ee.Image} cloud masked Sentinel-2 image */ function maskS2clouds(image) { var qa = image.select('QA60');

// Bits 10 and 11 are clouds and cirrus, respectively. var cloudBitMask = 1 << 10; var cirrusBitMask = 1 << 11;

// Both flags should be set to zero, indicating clear conditions. var mask = qa.bitwiseAnd(cloudBitMask).eq(0) .and(qa.bitwiseAnd(cirrusBitMask).eq(0));

return image.updateMask(mask).divide(10000); }

//Sentinel 2 dataset prefire var sentinel = ee.ImageCollection('COPERNICUS/S2_SR') .filterDate('2020-05-01', '2020-07-01') // Pre-filter to get less cloudy granules. .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',20)) .map(maskS2clouds);

var sentinelmean = sentinel.mean() var prefireCba=sentinelmean.clip(cordoba) //Sentinel 2 dataset post fire var sentinel2 = ee.ImageCollection('COPERNICUS/S2_SR') .filterDate('2020-09-01', '2020-10-25') // Pre-filter to get less cloudy granules. .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',20)) .map(maskS2clouds);

var sentinelmean2 = sentinel2.mean() var postfireCba=sentinelmean2.clip(cordoba)

var visualization = { min: 0.0, max: 0.3, bands: ['B12', 'B8', 'B4'] };

//Modis Dataset Thermal Anomalies var dataset = ee.ImageCollection('MODIS/006/MOD14A1') .filter(ee.Filter.date('2020-07-01', '2020-10-25'));

var scale=1000

var image =dataset.mean().clip(cordoba).select('MaxFRP'); var incendios = image.where(image.gte(1),1).reproject("EPSG:4326",null,1000);

//calculo NBR

var preNBR = prefireCba.normalizedDifference(['B8', 'B12']); var postNBR = postfireCba.normalizedDifference(['B8', 'B12']); var dNBR_unscaled = preNBR.subtract(postNBR); var dNBR = dNBR_unscaled.multiply(1000);

//----------------- Producto de severidad del incendio - Clasificación ----------------------

// Define un estilo SLD de intervalos discretos para aplicar a la imagen (paleta de color).

var sld_intervals = '' + '' + '' + '' + '' + '' + '' + '' + '' + '' + '';

var img1=dNBR.clip(zona1); var zona1 = img1.where(dNBR.gte(350),1).where(dNBR.lt(351),0).reproject("EPSG:4326",null,100); var img2=dNBR.clip(zona2); var zona2 = img2.where(dNBR.gte(150),1).where(dNBR.lt(151),0).reproject("EPSG:4326",null,100); var img3=dNBR.clip(zona3); var zona3 = img3.where(dNBR.gte(175),1).where(dNBR.lt(176),0).reproject("EPSG:4326",null,100); var img4=dNBR.clip(zona4); var zona4 = img4.where(dNBR.gte(220),1).where(dNBR.lt(221),0).reproject("EPSG:4326",null,100); var img5=dNBR.clip(zona5); var zona5 = img5.where(dNBR.gte(300),1).where(dNBR.lt(301),0).reproject("EPSG:4326",null,100); var img6=dNBR.clip(zona6); var zona6 = img6.where(dNBR.gte(275),1).where(dNBR.lt(276),0).reproject("EPSG:4326",null,100); var img7=dNBR.clip(zona7); var zona7 = img7.where(dNBR.gte(350),1).where(dNBR.lt(351),0).reproject("EPSG:4326",null,100); var img8=dNBR.clip(zona8); var zona8 = img8.where(dNBR.gte(325),1).where(dNBR.lt(326),0).reproject("EPSG:4326",null,100); var img9=dNBR.clip(zona9); var zona9 = img9.where(dNBR.gte(325),1).where(dNBR.lt(326),0).reproject("EPSG:4326",null,100); var img10=dNBR.clip(zona10); var zona10 = img10.where(dNBR.gte(200),1).where(dNBR.lt(201),0).reproject("EPSG:4326",null,100); var img11=dNBR.clip(zona11); var zona11 = img11.where(dNBR.gte(300),1).where(dNBR.lt(301),0).reproject("EPSG:4326",null,100); var img12=dNBR.clip(zona12); var zona12 = img12.where(dNBR.gte(300),1).where(dNBR.lt(301),0).reproject("EPSG:4326",null,100); var img13=dNBR.clip(zona13); var zona13 = img13.where(dNBR.gte(400),1).where(dNBR.lt(401),0).reproject("EPSG:4326",null,100); var img14=dNBR.clip(zona14); var zona14 = img14.where(dNBR.gte(350),1).where(dNBR.lt(350),0).reproject("EPSG:4326",null,100); var img15=dNBR.clip(zona15); var zona15 = img15.where(dNBR.gte(350),1).where(dNBR.lt(350),0).reproject("EPSG:4326",null,100); var img16=dNBR.clip(zona16); var zona16 = img16.where(dNBR.gte(350),1).where(dNBR.lt(350),0).reproject("EPSG:4326",null,100); var img18=dNBR.clip(zona18); var zona18= img18.where(dNBR.gte(350),1).where(dNBR.lt(350),0).reproject("EPSG:4326",null,100); var img17=dNBR.clip(zone17); var zona17= img17.where(dNBR.gte(300),1).where(dNBR.lt(301),0).reproject("EPSG:4326",null,100); var img19=dNBR.clip(zona19); var zona19= img19.where(dNBR.gte(350),1).where(dNBR.lt(351),0).reproject("EPSG:4326",null,100);

var mosaic = ee.ImageCollection([zona1,zona2,zona3,zona4,zona5,zona6,zona7,zona8,zona9,zona10,zona11,zona12,zona13,zona14,zona15,zona16,zona17,zona18,zona19]).mosaic().toInt();

var fuegoArea=ee.Image(1).mask(mosaic.eq(1));

//calculo area var area_pxa = fuegoArea.multiply(ee.Image.pixelArea()) .reduceRegion(ee.Reducer.sum(),cordoba,100,null,null,false,1e13) .get('constant');

area_pxa = ee.Number(area_pxa).divide(1e6);
print ('Area Quemada(has)', area_pxa.multiply(100));

// recortar imagenes detalles

var capillaPreFire =prefireCba.clip(CapilladelMonte); var capillaPostFire=postfireCba.clip(CapilladelMonte);

var panDeAzucarPreFire = prefireCba.clip(PanDeAzucar_LaCalera) var panDeAzucarPostFire = postfireCba.clip(PanDeAzucar_LaCalera)

//Exportar Imagenes a Google Drive Export.image.toDrive({ image: fuegoArea, description: 'incendios2020_sentinel100m', scale: 100, region: cordoba });

Export.image.toDrive({ image: capillaPreFire.select(['B12', 'B8', 'B4']), description: 'incendios2020_capillaDelMonte_pre', scale: 20, region: CapilladelMonte }); Export.image.toDrive({ image: capillaPostFire.select(['B12', 'B8', 'B4']), description: 'incendios2020_capillaDelMonte_post', scale: 20, region: CapilladelMonte });
Export.image.toDrive({ image: panDeAzucarPreFire.select(['B12', 'B8', 'B4']), description: 'incendios2020_panDeAzucar_pre', scale: 20, region: PanDeAzucar_LaCalera });
Export.image.toDrive({ image: panDeAzucarPostFire.select(['B12', 'B8', 'B4']), description: 'incendios2020_panDeAzucar_ost', scale: 20, region: CapilladelMonte });

Map.setCenter(-64.472086,-30.846342, 8);

Map.addLayer(fuegoArea,{}, 'Area Incendiada');

//Map.addLayer(dNBR.sldStyle(sld_intervals), {}, 'dNBR clasificado');

//Map.addLayer(cordoba, {palette: 'FF0000'}, 'Cordoba'); //Map.addLayer(incendios,{palette:"ff0000"}, 'Incendios'); Map.addLayer(panDeAzucarPreFire, visualization, 'preincendio'); Map.addLayer(panDeAzucarPostFire, visualization, 'post incendio'); Map.addLayer(capillaPreFire, visualization, 'preincendio'); Map.addLayer(capillaPostFire, visualization, 'post incendio');
