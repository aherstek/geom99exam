# GEOM99 Exam - Part 1 Notes and Documentation

**Problem Statement**
* Currently, a fictitious Local Government in Ontario creates and prints paper maps of the surveyed boundaries for Real Estate Agents in support of properties for sale. These maps cost real estate agents $60 per request and most just shared as custom-created PDFs from a desktop GIS. The Real Estate Agents are requesting the Local Government information be easier to access though a website and (ideally) would cost less per request. They would prefer a single-fee per year for all requests, versus paying per requests as well. Currently there are about 200 paid map requests per year, or about $12,000 in fees collected per year for the roughly 50 real-estate agents in the city. 
* You (as a newly-graduated GIS professional) are to propose a new web-GIS solution that would not cost more than the revenue generated per year over 3 years to run and maintain, meeting the needs identified (sharing parcel property maps).  Do not include labour in costs creating the solution. This is only a rough proposal so do not detail implementation steps. However you must be technically specific and your description must align with how the proposed technology works and how it was taught in the class. 
* Answer the questions about your solution which meet these above needs given the following criteria and resources identified:
  * Currently some individuals in the local government use QGIS as GIS desktop software, but are no experts. They are open to switch to commercial software if they can have it funded by this solution.
  * The local government has no server infrastructure and no capacity to host web sites or servers. Their IT department is minimal and only supports windows desktop machines.
  * The real estate agents need to see parcel ownership boundaries (4,000 parcels polygon records) over some type of aerial image. 
  * The parcels data from the province are available as a GeoPackage download or as an ArcGIS Server Map Service, your choice. The solution must be updated at least each week with a fresh parcel layer (if necessary). 
  * The city also has data for roads (2,200-line segments), parks (23 polygons), address points (5,500 points), which are updated a no more than 6 times per year and would be ideal to show those on the map as well.
  * If there are any ongoing (monthly/yearly) costs for the proposed solution, Real Estate Agents will need to pay a fee for access to the proposed system. This can recover ongoing monthly costs incurred by the local government. If any monthly costs exists there must be a way to create accounts (security) for each agent to log in on the system to manage access. An estimate of costs is fine, exact figures are not necessary. 
* Research a solution meeting these requirements before the exam and create some notes to answer questions on how your proposed solution will work, its limitations, costs and technical overview. Point form will be fine for answers of questions provided during the exam and you will have access to your notes, the internet and any other resources during this portion of the exam.

# General Notes
* Simcoe County serves as my inspiration for this solution - I remember exploring this web solution after Shawn introduced it in class (Week 11) and I want to try to recreate an open source solution to fit this local governments needs and requirements.
 * Links to Simcoe County provided in class: https://github.com/county-of-simcoe-gis & https://opengis.simcoe.ca/
* After exploring Simcoe County's WebGIS solution - I believe that it can be utilized as a baseline idea for this problem statement. Searching for parcel boundaries, roads, parks and addresses, having aerial imagery as a basemap, and all are possible in this solution - which fits our clients criteria. 
* <img width="619" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/a7644c17-1c15-4b6f-8ffd-146614202c43">

# Database / Data Tier - **PostgreSQL/PostGIS**

## PostgreSQL - https://www.postgresql.org/

* _Explored code repository: https://pgpedia.info/ & https://github.com/dhamaniasad/awesome-postgres_

* Open source object-relational database system

## PostGIS - https://postgis.net/

* _Explored code repository: https://git.osgeo.org/gitea/postgis/postgis_

* Extends PostgreSQL capabilities by adding support for storing, indexing, and querying geospatial data.
* PostGIS features include:
  * Spatial Data Storage: Store different types of spatial data such as points, lines, polygons, and multi-geometries, in both 2D and 3D data.
  * Spatial Indexing: Quickly search and retrieve spatial data based on its location.
  * Spatial Functions: A wide range of spatial functions that allow you to filter and analyze spatial data, measuring distances and areas, intersecting geometries, buffering, and more.
  * Geometry Processing: Tools for processing and manipulating geometry data, such as simplification, conversion, and generalization.
  * Raster Data Support: Storage and processing of raster data, such as elevation data and weather data.
  * Geocoding and Reverse Geocoding: Functions for geocoding and reverse geocoding.
  * Integration: Access and work with PostGIS using third party tools such as QGIS, GeoServer, MapServer, ArcGIS, Tableau.

# Server / Middle Tier - **GeoServer and Google Cloud Platform**

## GeoServer - https://geoserver.org/

## GCP - https://cloud.google.com/?hl=en

* **Google Cloud's pricing calculator: https://cloud.google.com/products/calculator/?hl=en**
* My attempt to calculate how much running a GCP VM with storage might be:
  * ~4000 parcel shapefiles updated ~1 / week
    * Rounding up to say it takes 1 day a week to complete, 8 hours x 4 = 32 hours a month on VM
  * Roads (2,200-line segments), parks (23 polygons), address points (5,500 points) updated ~6 / year
    * Rounding up to say this takes 2 days to complete, 16 x 6 = 96 hours a year / 12 months = 8 hours a month

  * Both of these tasks add up to 40 hours a month using a VM - I have chosen 1 instance
  * When putting this into the calculator, it says it would be 0.05 of an instance
  * 1 instance = 730 hours ($138.70/month) - this could be the way to go?
     * includes 15gb memory/RAM, 4 vCPUs, machine type: n1-standard-4
  * * <img width="890" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/638fa8d0-14a3-4f27-aef3-24347368ce84">
   
  * I have also opted to add Networking for security (10 IP addresses allocated for VM) and Cloud Storage (5000 GB)
  * <img width="886" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/dece94af-ffef-475a-97a6-d92f3c56b717">

* Grand total: **$374.07/month or $4488.84/year**

 



# Client / WebGIS Tier - **Open Layers**

## Open Layers - https://openlayers.org/

* _Explored code repository: https://github.com/openlayers/openlayers_


# How It All Works Together

## <u>PostgreSQL/PostGIS</u>
### Spatial data / databases is stored here (hosted through GCP)

## GeoServer
### Take data from the database and pushes it to Open Layers upon request. 

## Open Layers
### Receives map data from GeoServer and users can interact with the map.


# Limitations / Things I Don't Know
## Editing Data
* Not sure how to implement data editing capabilities entirely 
  * Have researcged using tools like OpenLayers or by GeoServer extensions? If users made changes to the spatial data, would these changes be reflected in the PostgreSQL/PostGIS database?
