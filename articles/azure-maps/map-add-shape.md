---
title: Add a polygon layer to a map | Microsoft Azure Maps
description: Learn how to add polygons or circles to maps. See how to use the Azure Maps Web SDK to customize geometric shapes and make them easy to update and maintain.
author: dubiety
ms.author: yuchungchen
ms.date: 06/07/2023
ms.topic: how-to
ms.service: azure-maps
---

# Add a polygon layer to the map

This article shows you how to render the areas of `Polygon` and `MultiPolygon` feature geometries on the map using a polygon layer. The Azure Maps Web SDK also supports the creation of Circle geometries as defined in the [extended GeoJSON schema](extend-geojson.md#circle). These circles are transformed into polygons when rendered on the map. All feature geometries can easily be updated when wrapped with the [atlas.Shape](/javascript/api/azure-maps-control/atlas.shape) class.

## Use a polygon layer

When a polygon layer is connected to a data source and loaded on the map, it renders the area with `Polygon` and `MultiPolygon` features. To create a polygon, add it to a data source, and render it with a polygon layer using the [PolygonLayer](/javascript/api/azure-maps-control/atlas.layer.polygonlayer) class.

The following sample code demonstrates creating a polygon layer that covers New York City's Central Park with a red polygon.

```javascript
 
function InitMap()
{
  var map = new atlas.Map('myMap', {
    center: [-73.97, 40.78],
    zoom: 11,
    view: "Auto",
    
    //Add authentication details for connecting to Azure Maps.
    authOptions: {
      authType: 'subscriptionKey',
      subscriptionKey: '{Your-Azure-Maps-Subscription-key}'
    }
  });

  //Wait until the map resources are ready.
  map.events.add('ready', function () {

    /*Create a data source and add it to the map*/
    var dataSource = new atlas.source.DataSource();
    map.sources.add(dataSource);

    /*Create a rectangle*/
    dataSource.add(new atlas.Shape(new atlas.data.Feature(
    new atlas.data.Polygon([[
      [-73.98235, 40.76799],
      [-73.95785, 40.80044],
      [-73.94928, 40.7968],
      [-73.97317, 40.76437],
      [-73.98235, 40.76799]
    ]])
    )));

    /*Create and add a polygon layer to render the polygon to the map*/
    map.layers.add(new atlas.layer.PolygonLayer(dataSource, null,{
    fillColor: "red",
    fillOpacity: 0.7
    }), 'labels')
  });
}

```

:::image type="content" source="./media/map-add-shape/polygon-layer.png" alt-text="A screenshot of map of New York City demonstrating the polygon layer covering all of Central Park with fill Color set to red and fill Opacity set to 0.7.":::

  <!--------------------------------------------------------------------------
<iframe height='500' scrolling='no' title='Add a polygon to a map ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true'>See the Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Add a polygon to a map </a> by Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
------------------------------------------------------------------------->

## Use a polygon and line layer together

A line layer is used to render the outline of polygons. The following code sample renders a polygon like the previous example, but now adds a line layer. This line layer is a second layer connected to the data source.  

```javascript
function InitMap()
{
  var map = new atlas.Map('myMap', {
    center: [-73.97, 40.78],
    zoom: 11,
    view: "Auto",
    
    //Add authentication details for connecting to Azure Maps.
    authOptions: {
      // Get an Azure Maps key at https://azuremaps.com/.
      authType: 'subscriptionKey',
      subscriptionKey: '{subscription key}'
    }
  });

  //Wait until the map resources are ready.
  map.events.add('ready', function () {

    /*Create a data source and add it to the map*/
    var dataSource = new atlas.source.DataSource();
    map.sources.add(dataSource);

    /*Create a rectangle*/
    dataSource.add(new atlas.data.Polygon([[
    [-73.98235, 40.76799],
    [-73.95785, 40.80045],
    [-73.94928, 40.7968],
    [-73.97317, 40.76437],
    [-73.98235, 40.76799]
    ]])
          );

    //Create a polygon layer to render the filled in area of the polygon.
    var polygonLayer = new atlas.layer.PolygonLayer(dataSource, 'myPolygonLayer', {
    fillColor: 'rgba(0, 200, 200, 0.5)'
    });

    //Create a line layer for greater control of rendering the outline of the polygon.
    var lineLayer = new atlas.layer.LineLayer(dataSource, 'myLineLayer', {
    strokeColor: 'red',
    strokeWidth: 2
    });

    /*Create and add a polygon layer to render the polygon to the map*/
    map.layers.add([polygonLayer, lineLayer])
  });
}
```

:::image type="content" source="./media/map-add-shape/polygon-line-layer.png" alt-text="A screenshot of a map of New York City demonstrating a mostly transparent polygon layer covering all of Central Park, bordered with a red line.":::

<!------------------------------------------------------------
<iframe height='500' scrolling='no' title='Polygon and line layer to add polygon' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true'>See the Pen <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>Polygon and line layer to add polygon</a> by Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
------------------------------------------------------------>

## Fill a polygon with a pattern

In addition to filling a polygon with a color, you may use an image pattern to fill the polygon. Load an image pattern into the maps image sprite resources and then reference this image with the `fillPattern` property of the polygon layer.

For a fully functional sample that shows how to use an image template as a fill pattern in a polygon layer, see [Fill polygon with built-in icon template] in the [Azure Maps Samples]. For the source code for this sample, see [Fill polygon with built-in icon template source code].

:::image type="content" source="./media/map-add-shape/fill-polygon-with-built-in-icon-template.png" alt-text="A screenshot of a map of the world with red dots forming a triangle in the center of the map.":::

<!------------------------------------------------------------
<iframe height="500" scrolling="no" title="Polygon fill pattern" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder='no' loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>Polygon fill pattern</a> by Azure Maps
  (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
------------------------------------------------------------>

> [!TIP]
> The Azure Maps web SDK provides several customizable image templates you can use as fill patterns. For more information, see the [How to use image templates](how-to-use-image-templates-web-sdk.md) document.

## Customize a polygon layer

The Polygon layer only has a few styling options. See the [Polygon Layer Options] sample map in the [Azure Maps Samples] to try them out.

:::image type="content" source="./media/map-add-shape/polygon-layer-options.png" alt-text="A screenshot of the Polygon Layer Options tool.":::

<!------------------------------------------------------------
<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true'>See the Pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> by Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
------------------------------------------------------------>

<a id="addACircle"></a>

## Add a circle to the map

Azure Maps uses an extended version of the GeoJSON schema that provides a [definition for circles](extend-geojson.md#circle). A circle is rendered on the map by creating a `Point` feature. This `Point` has a `subType` property with a value of `"Circle"` and a `radius` property with a number that represents the radius in meters.

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}  
```

The Azure Maps Web SDK converts these `Point` features into `Polygon` features. Then, these features are rendered on the map using polygon and line layers as shown in the following code sample.

```javascript
function InitMap()
{
  var map = new atlas.Map('myMap', {
    center: [-73.985708, 40.75773],
    zoom: 12,
    view: "Auto",
    
    //Add authentication details for connecting to Azure Maps.
    authOptions: {
      // Get an Azure Maps key at https://azuremaps.com/.
      authType: 'subscriptionKey',
      subscriptionKey: '{Your-Azure-Maps-Subscription-key}'
    }      
  });

  //Wait until the map resources are ready.
  map.events.add('ready', function () {

    /*Create a data source and add it to the map*/
    var dataSource = new atlas.source.DataSource();
    map.sources.add(dataSource);

    //Create a circle
    dataSource.add(new atlas.data.Feature(new atlas.data.Point([-73.985708, 40.75773]), 
    {
      subType: "Circle",
      radius: 1000
    }));

    // Create a polygon layer to render the filled in area
    // of the circle polygon, and add it to the map.
    map.layers.add(new atlas.layer.PolygonLayer   (dataSource, null, {
      fillColor: 'rgba(0, 200, 200, 0.8)'
    }));
  });
}
```

:::image type="content" source="./media/map-add-shape/add-circle-to-map.png" alt-text="A screenshot of a map showing a partially transparent green circle in New York City. This demonstrates adding a circle to a map.":::

  <!---------------------------------------------------------
<iframe height='500' scrolling='no' title='Add a circle to a map' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true'>See the Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>Add a circle to a map</a> by Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
---------------------------------------------------------->
## Make a geometry easy to update

A `Shape` class wraps a [Geometry](/javascript/api/azure-maps-control/atlas.data.geometry) or [Feature](/javascript/api/azure-maps-control/atlas.data.feature) and makes it easy to update and maintain these features. To instantiate a shape variable, pass a geometry or a set of properties to the shape constructor.

```javascript
//Creating a shape by passing in a geometry and a object containing properties.
var shape1 = new atlas.Shape(new atlas.data.Point[0,0], { myProperty: 1 });

//Creating a shape using a feature.
var shape2 = new atlas.Shape(new atlas.data.Feature(new atlas.data.Point[0,0], { myProperty: 1 });
```

The [Make a geometry easy to update] sample shows how to wrap a circle GeoJSON object with a shape class. As the value of the radius changes in the shape, the circle renders automatically on the map. For the source code for this sample, see [Make a geometry easy to update source code].

:::image type="content" source="./media/map-add-shape/easy-to-update-geometry.png" alt-text="A screenshot of a map showing a red circle in New York City with a slider bar titled Circle Radius and as you slide the bar to the right or left, the value of the radius changes and the circle size adjusts automatically on the map.":::

  <!---------------------------------------------------------
<iframe height='500' scrolling='no' title='Update shape properties' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true'>See the Pen <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>Update shape properties</a> by Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
---------------------------------------------------------->

## Next steps

Learn more about the classes and methods used in this article:

> [!div class="nextstepaction"]
> [Polygon](/javascript/api/azure-maps-control/atlas.data.polygon)

> [!div class="nextstepaction"]
> [PolygonLayer](/javascript/api/azure-maps-control/atlas.layer.polygonlayer)

> [!div class="nextstepaction"]
> [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions)

For more code examples to add to your maps, see the following articles:

> [!div class="nextstepaction"]
> [Create a data source](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Add a popup](map-add-popup.md)

> [!div class="nextstepaction"]
> [Use data-driven style expressions](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [How to use image templates](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Add a line layer](map-add-line-layer.md)

Additional resources:

> [!div class="nextstepaction"]
> [Azure Maps GeoJSON specification extension](extend-geojson.md#circle)

[Fill polygon with built-in icon template]: https://samples.azuremaps.com/?sample=fill-polygon-with-built-in-icon-template
[Azure Maps Samples]: https://samples.azuremaps.com
[Make a geometry easy to update]: https://samples.azuremaps.com/?sample=make-a-geometry-easy-to-update
[Fill polygon with built-in icon template source code]: https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/main/Samples/Polygons/Fill%20polygon%20with%20built-in%20icon%20template/Fill%20polygon%20with%20built-in%20icon%20template.html
[Make a geometry easy to update source code]: https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/main/Samples/Polygons/Make%20a%20geometry%20easy%20to%20update/Make%20a%20geometry%20easy%20to%20update.html
