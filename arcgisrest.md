# ArcGIS REST api introduction

This site introduces the ArcGIS REST API to those learning about web GIS. In particular, it will focus on navigating and using this documentation: https://developers.arcgis.com/rest/services-reference/enterprise/get-started-with-the-services-directory.htm

## What is REST anyway?

Representational State Transfer (https://en.wikipedia.org/wiki/Representational_state_transfer) is a common web architecture for middle-ware that uses URLs to retrieve data from internal data systems. Each URL request to a REST endpoint (as they are called) is independent of any other request (stateless). This means the server providing the results to a REST endpoint request is not concerned with what it did before, or after; it just needs to work on the current request. It is the job of an application to maintain its state (for example, remember where you are looking when panning on a map). 

Refresher: What is a URL? https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL

The modern development pattern for web solutions is a web-tier, which is only the user interface, a data-tier that powers the information usually required for applications, and some middle stuff that bridges the gap between the user and data. REST is ideal for this, especially compared to other technologies like SOAP and XML which are hard to navigate interpret as a programmer. This is where most REST endpoints shine: They have a "human" navigable GUI. 

Most REST endpoints have two versions, one focused on the developers (humans), and another focused on the computer (particularly but not limited to JavaScript). If you open the "human" version you get a common website interface with links and lists. It is well organized, but isn't a fancy website like a marketing page. It is utilitarian and functional to server its purpose: it provides a clickable way you can interact with the interface, test, design new URLs that do specific use cases, and then possible program something to generate the same URLs and automate a process or task. 

## ArcGIS Enterprise

ArcGIS Enterprise is a software suite and web-GIS platform. The two main components, ArcGIS Server and Portal, both have REST endpoints to manage its configuration. ArcGIS Server is the main spatial engine for ArcGIS Enterprise, and all data published through Portal or directly against ArcGIS Server are made available in a rich and well-documented REST endpoint called the ArcGIS REST API.

## ArcGIS Server

Learning to navigate ArcGIS Server's REST API gives data administrators a powerful tool in discovering what capabilities are available, debugging problems with solutions, and ensuring data shared are secured, appropriate and valid. Without understanding how to navigate the REST API, an administer is effectively working blind and only can use the GUI within ArcGIS Server or Portal to see what is happening with their environment.

## The REST Services Directory

The ArcGIS REST services directory is a website (HTML version) and programming interface (JSON Version) to published services. This is often called an endpoint. The default location for this rest endpoint is in a path of `/arcgis/rest/services`. The "arcgis" portion can be configured and sometimes there are more folders placed in the path, but for ESRI it always has `/rest/services` when dealing with Data and Map Services, which this learning tool focuses on. 

1. Use Google to search for a place and `/arcgis/rest/services` and see the various public services

Search "ontario arcgis/rest/services": https://www.google.com/search?q=ontario+%2Farcgis%2Frest%2Fservices

Many of the returned searches are for government ArcGIS REST endpoints that power much of the public websites! All of these can be read by a program or person. How? Each is available both as a JSON and HTML version:

- Example HTML Version: https://sampleserver6.arcgisonline.com/arcgis/rest/services
- Example JSON Version: https://sampleserver6.arcgisonline.com/arcgis/rest/services?f=pjson

What is different between the two above URLS? 

<details><summary>Click for Answer</summary>

- There is no URL parameter on the first one, but the second one specifically asks for the returned results in JSON. The JSON version is meant to be serializable by a program, usually JavaScript. That is just a fancy way of saying a programmer can code something to read and navigate the information shown on the REST endpoint easily. 
- Why the P in front of JSON? "Pretty" JSON. Pretty? It is formatted nicely to be read by a human and program.

</details>

2. Open the less "pretty" version of the interface by changing the f=pjson to be f=json in the URL. 

Now the JSON bunches up (all spaces are removed) and is arguably less easily read by us people? This is an optimum format for a program to read, but not very easily read as a person. The PJSON version is easier for people, but without question the HTML version is easiest for us to read since it even has links that can be clicked. 

3. Open the HTML version by changing the f=json in the URL to be f=html

The f=html option is the default, so if the "f" URL parameter left off it will show the html version. There are other output formats, but for the purposes here we will be focusing on the HTML version. The full documentation for this URL parameter is at https://developers.arcgis.com/rest/services-reference/enterprise/output-formats.htm.

4. Return your focus to and open just the HTML version at https://sampleserver6.arcgisonline.com/arcgis/rest/services. The remaining parts of the exercise will always use the HTML version, but remember you can always change the format to `f=json` or `f=pjson` in the URL parameters to return JSON or PJSON. 

Now, using the HTML version of sampleserver6 (PS: there is a sampleserver5 as well, with the same content) let us explore some functionality with existing published map services.

## Navigating the MapServer REST Endpoint

Full API Documentation: https://developers.arcgis.com/rest/services-reference/enterprise/map-service.htm

A MapServer is available on ArcGIS Enterprise and ArcGIS Server environments, but not ArcGIS Online. This contains data (layers) combined together into a single "map" with default applied cartography. Think of it as a pre-created map that you can easily request as an image. This is called a Dynamic Map, meaning that the image provided is generated using the live data on the server. 

Example: https://sampleserver6.arcgisonline.com/arcgis/rest/services/USA/MapServer/export?bbox=-89.25%2C35.48%2C-72.61%2C46.01&size=713%2C451&format=png32&f=image

Generates this image:

![map example](https://sampleserver6.arcgisonline.com/arcgis/rest/services/USA/MapServer/export?bbox=-89.25%2C35.48%2C-72.61%2C46.01&size=450%2C200&format=png32&f=image)

Lets break this down into its method and parameters, section by section (remember, URL parameter order does not matter when submitting a URL for a REST request and `%2C` is an encoded `,` (comma)):

- Each parameter below is detailed in the documentation, and is recommended you read each listed below from here: https://developers.arcgis.com/rest/services-reference/enterprise/export-map.htm

### EXPORT method

The Export is a method (it does something) and generates the image on the server then returns the image or a link to the image with the client as the format requested. 

### Bounding Box `bbox=-89.25,35.48,-72.61,46.01`

This is the bounding box for the request. The image will be generated using the geographical box indicated. The syntax is `<xmin>, <ymin>, <xmax>, <ymax>` using the spatial reference of the map (or converted, if a different spatial reference is used by including the URL parameter `bbSR`). 

### Image Size `size=450,200`

Given the bounding box, this image size request will be the size of the actual image returned in pixels `<width>,<height>`.

### Image Format `format=png32`

The image format to be returned can be specified using this parameter. In this case, it is a 32-bit PNG that is created and returned. There are a lot of options for this, including any one of: 

`png | png8 | png24 | jpg | pdf | bmp | gif | svg | svgz | emf | ps | png32`

### Return the image `f=image`

This option just returns the image directly from the request. It can be used in an `<img src="image">` tag directly, where the image is the entire URL that generates that static image. There is no user interface around this image being displayed, so you cannot interact with the map. 

### What else can a MapServer do?

The URL example for export just returns an image. You cannot use the image to do anything, other than look! So there are other methods on the MapServer that allow you to interact with the data, including identify. Here is an identify example all pre-populated asking for a point on a map of the USA right on the state line between Nevada and California (note some characters are not URL encoded for easier readability, but they should be if using this in a production environment!):

https://sampleserver6.arcgisonline.com/arcgis/rest/services/USA/MapServer/identify?geometryType=esriGeometryPoint&geometry=-120,40&tolerance=10&mapExtent=-119,38,-121,41&imageDisplay=400,300,96

You can actively change any of the values and re-run the query with results returning back below the form. It is a nice way to play and test out what is happening, as well to learn what the capabilities are. Refer to the API documentation (https://developers.arcgis.com/rest/services-reference/enterprise/identify-map-service-.htm) if you want to learn what each parameter expects, which are required, and what format should they be written in. 

Identify and Export are just two methods available on the MapServer REST endpoint. Click on the `USA (MapServer)` at the top of the REST Endpoint HTML window to return to the main MapServer interface. Scroll to the bottom where it has Supported Operations shown. This is a clickable list of all the methods available for this service! You can learn about each by clicking and looking at the form to drive the interface.

## Navigating the FeatureServer REST Endpoint

The Feature Service allows you to interact with the actual vector geometries and is much more capable than the Map Service. 
