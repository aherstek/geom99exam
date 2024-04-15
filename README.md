# GEOM99 Exam - Part 1 Notes and Documentation

## **Problem Statement**
* Currently, a fictitious Local Government in Ontario creates and prints paper maps of the surveyed boundaries for Real Estate Agents in support of properties for sale. These maps cost real estate agents $60 per request and most just shared as custom-created PDFs from a desktop GIS. The Real Estate Agents are requesting the Local Government information be easier to access though a website and (ideally) would cost less per request. They would prefer a single-fee per year for all requests, versus paying per requests as well. Currently there are about 200 paid map requests per year, or about $12,000 in fees collected per year for the roughly 50 real-estate agents in the city. 
* You (as a newly-graduated GIS professional) are to propose a new web-GIS solution that would not cost more than the revenue generated per year over 3 years to run and maintain, meeting the needs identified (sharing parcel property maps).  Do not include labour in costs creating the solution. This is only a rough proposal so do not detail implementation steps. However you must be technically specific and your description must align with how the proposed technology works and how it was taught in the class. 
* Answer the questions about your solution which meet these above needs given the following criteria and resources identified:
  * Currently some individuals in the local government use QGIS as GIS desktop software, but are no experts. They are open to switch to commercial software if they can have it funded by this solution.
  * :key: I am choosing to stick with open source data, but not QGIS. I will be creating my own solution modelled after Simcoe County's - utilizing a variety of software.
  * The local government has no server infrastructure and no capacity to host web sites or servers. Their IT department is minimal and only supports windows desktop machines.
  * :key: I will be implementing Google Cloud Platform to host a VM instance as their server and consequently, GeoServer.
  * :key: Using GCP VM would not necessarily require them to have their own physical server infrastructure, as a GCP VM can be managed by them remotely (access via internet) - no maintenance of their own server hardware per say.
  * The real estate agents need to see parcel ownership boundaries (4,000 parcels polygon records) over some type of aerial image.
  * :key: OpenLayers has the ability to display vmap tiles, vector data and markers loaded from any source as well as various basemaps, including aerial imagery.
  * The parcels data from the province are available as a GeoPackage download or as an ArcGIS Server Map Service, your choice. The solution must be updated at least each week with a fresh parcel layer (if necessary).
  * :key: This solution will use a GeoPackage download. Working in the GCP VM can allow for updates in data.
  * The city also has data for roads (2,200-line segments), parks (23 polygons), address points (5,500 points), which are updated a no more than 6 times per year and would be ideal to show those on the map as well.
  * :key: GCP allows for these updates to occur.
  * If there are any ongoing (monthly/yearly) costs for the proposed solution, Real Estate Agents will need to pay a fee for access to the proposed system. This can recover ongoing monthly costs incurred by the local government. If any monthly costs exists there must be a way to create accounts (security) for each agent to log in on the system to manage access. An estimate of costs is fine, exact figures are not necessary.
  * :key: GCP cost breakdown below (~$4400/annual and this is with a network security option included)
* Research a solution meeting these requirements before the exam and create some notes to answer questions on how your proposed solution will work, its limitations, costs and technical overview. Point form will be fine for answers of questions provided during the exam and you will have access to your notes, the internet and any other resources during this portion of the exam.

# April 15 11:50am Addendum

* Rereading the requirements... I think I may have misinterpreted/overlooked _"no capacity to host web sites or servers"_ - which in that case eliminates my domain idea... or maybe i'm okay? Now I'm second guessing my solution :cry:
* Utilizing QGIS as a desktop software I think would still work?
* QGIS website documentation: https://www.qgis.org/en/site/ / https://www.qgis.org/en/docs/index.html
* It is possible to use QGIS with GeoServer: https://www.youtube.com/watch?v=DuZFjBPHtDE
* Also has an OpenLayers plug in: https://www.youtube.com/watch?v=TvzYAg2Zvek
* QGIS OpenLayers plug in via Python: https://plugins.qgis.org/plugins/openlayers_plugin/



_________________________________________
# General Notes
* Simcoe County serves as my inspiration for this solution - I remember exploring this web solution after Shawn introduced it in class (Week 11) and I want to try to recreate an open source solution to fit this local governments needs and requirements.
 * Links to Simcoe County provided in class: https://github.com/county-of-simcoe-gis & https://opengis.simcoe.ca/
* After exploring Simcoe County's WebGIS solution - I believe that it can be utilized as a baseline idea for this problem statement. Searching for parcel boundaries, roads, parks and addresses, having aerial imagery as a basemap, and all are possible in this solution - which fits our clients criteria. 
* <img width="619" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/a7644c17-1c15-4b6f-8ffd-146614202c43">

_________________________________________
# Database / Data Tier - **PostgreSQL/PostGIS**

## PostgreSQL - https://www.postgresql.org/
:moneybag: Free to download, open source

* _Explored code repository: https://pgpedia.info/ & https://github.com/dhamaniasad/awesome-postgres_

* Open source object-relational database system

## PostGIS - https://postgis.net/
:moneybag: Free to download, open source

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

_________________________________________
# Server / Middle Tier - **GeoServer and Google Cloud Platform**

## GeoServer - https://geoserver.org/
:moneybag: Free to download, open source - can pay for extensions

* Can be configured to talk to PostgreSQL/PostGIS to retrieve data and push it to OpenLayers map upon request / when queried.
* Acts as middle tier to retrieve spatial data (from database) and display it for the user (client side)

## GCP - https://cloud.google.com/?hl=en
:moneybag: Not free - cost breakdown to follow below

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

_________________________________________
# Client / WebGIS Tier - **OpenLayers**

## OpenLayers - https://openlayers.org/
:moneybag: Free, using open sources, JavaScript library / APIs

* _Explored code repository: https://github.com/openlayers/openlayers_

### How data might be displayed:

 * **Aerial imagery**:
   * Searching ol/source provides you with many options to find an aerial basemap that suits your needs, here are 2 examples:
      * ol/source/OSM - https://openlayers.org/en/latest/apidoc/module-ol_source_OSM-OSM.html
      * ol/source/Google - https://openlayers.org/en/latest/apidoc/module-ol_source_Google-Google.html
      * <img width="947" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/630996db-c143-4eab-bc5d-66b3943af507">
      * <img width="387" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/cd8ba113-8f89-46b3-9bad-bfe7b74d2456">

* **Parcel ownership:**
   * ol/layer/vector -  https://openlayers.org/en/latest/apidoc/module-ol_layer_Vector-VectorLayer.html -- displays vector layer
   * ol/source/vector -  https://openlayers.org/en/latest/apidoc/module-ol_source_Vector-VectorSource.html -- displays vector source info / metadata

* **Roads, parks and address points:**
   * ol/layer/vector -  https://openlayers.org/en/latest/apidoc/module-ol_layer_Vector-VectorLayer.html -- displays vector layer
   * ol/source/vector - https://openlayers.org/en/latest/apidoc/module-ol_source_Vector-VectorSource.html -- displays vector source info / metadata

* **Other useful additions:**
* There are _tons_ of ways you can customize you map, here are 3 I found through the documentation:
   * ol/interaction: https://openlayers.org/en/latest/apidoc/module-ol_interaction_Interaction.html -- allows zooming, panning, etc
   * ol/style: https://openlayers.org/en/latest/apidoc/module-ol_style_Style-Style.html -- change appearance of vector data
   * ol/interaction/select: https://openlayers.org/en/latest/apidoc/module-ol_interaction_Select-Select.html -- select features on map to see more info about it 

### Domain
:moneybag: Not free, cost breakdown below
* **To create a domain (if needed): https://easydns.com/domains/register-ca-domain/**
   * ~$20 / year to maintain

_________________________________________
# How It All Works Together (I think?)

<img width="652" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/ad43d812-5b8e-4e90-bcf8-f2b6bf38af4c">

^ and then hosting it on a domain (forgot that part in my flow chart)

## PostgreSQL/PostGIS

### Spatial data / databases is stored here (hosted through GCP).

## GeoServer

### Take data from the database and pushes it to OpenLayers upon request. 

## Open Layers

### Receives map data from GeoServer and users can interact with the map.

## Google Cloud Platform

### Hosts PostgreSQL/PostGIS and GeoServer

_________________________________________
# Technical Steps To Set Up Solution

1. _In GCP (example to get set up)_
    * Create an account
    * Create an instance
    * Install PostgreSQL/PostGiS and GeoServer on VM
    * Set up firewall rules
       * port 5432 for PostgreSQL, port 8080 for GeoServer, and port 80 or 443 for HTTP/HTTPS (I think?)

2. _In PostgreSQL/PostGIS (example to get set up)_
    * Install PostgreSQL on VM
    * Add PostGIS extension
    * Create new database
    * Import provided GeoPackage data from client
    * QAQC to verify data uploaded correctly

 3. _In GeoServer (example to get set up)_
    * Download GeoServer on VM
    * Configure the data store (i think?) to connect to PostgreSQL database
    * Publish layers:
    * > Set up workspaces > new vector data source > browse to location > save
    * > Layer preview = see uploaded layers
    * > Under "all formats" test data is working with GeoJSON

4.   _In Open Layers (with domain) (example to get set up)_
     * Set up web server (domain creation)
     * Create HTML page (could use OSM base layer discussed above, etc)
     * Connect GeoServer layers (WMS or WFS? not sure)
     * Upload site to server
     * Access OpenLayers map - test query to ensure data is pulling correctly

_________________________________________
# Limitations / Things I Don't Know / Other Thoughts
## Editing Data - is possible, I just don't completely know the workflow yet
* Not sure how to implement data editing capabilities entirely 
  * Have researcged using tools like OpenLayers or by GeoServer extensions? If users made changes to the spatial data, would these changes be reflected in the PostgreSQL/PostGIS database?

## Support / Troubleshooting
* Utilizing _many_ different softwares / services means that if some portion of it is not working / need help figuring something out - you need to determine the source to talk to the appropriate helpline
   * Paid softwares like ESRI have readily accessible help, open source may be a slower process? ESRI built to work seamlessly (in theory, lol) together while combining open source softwares together may be more complex to fix problems in.

## Authentication / Authorization / Security
* Not sure where to implement a username/password/etc for security... logging into VM is secure I think (only 10 authorized users)
* Unsure what firewall settings should be used.
* I think GeoServer and PostgreSQL have security configurations?
    * GeoServer features = HTTPS support, authentication, access control, OAuth (from website)
    * Can also set up passwords in GeoSever/OpenLayers config?
OpenLayers Security: Secure your OpenLayers application by using HTTPS and implementing proper authentication and authorization mechanisms if required.
* I did find IAM (Identity and Access Management) through researching GCP but I am unaware if this suits the clients requirements - https://cloud.google.com/security/products/iam
   * IAM manages access control by defining who (identity) has what access (role) for which resource.
   * :moneybag: ^ free service through GCP

_________________________________________
# Research / Sources Used
* JDBC and Geoserver - https://docs.geoserver.org/main/en/user/community/jdbcconfig/configuration.html
* GeoServer and PostgreSQL compatibility - https://gis.stackexchange.com/questions/413984/finding-compatibility-of-geoserver-versions-to-postgresql-postgis
* Put an OpenLayers map on a webpage - https://openlayers.org/doc/quickstart.html
* GeoServer and PostGIS - https://docs.geoserver.org/main/en/user/data/database/postgis.html
* GeoPackage: getting started: https://www.geopackage.org/guidance/getting-started.html
* Developing web solutions using open source - https://www.arcgis.com/apps/Cascade/index.html?appid=fc87e9216339415aadeb7d694137bd27
* Simcoe County and OpenLayers: https://github.com/county-of-simcoe-gis/SimcoeCountyWebViewer/blob/master/README.md
* Simcoe County deployment guide: https://github.com/county-of-simcoe-gis/SimcoeCountyDeploymentGuide
* Shawn's "Reduce ArcGIS Online credit usage by having Geoserver host your data!": https://www.youtube.com/watch?v=2y4pEC-Cj6I
* Setting up PostgreSQL database: https://www.microfocus.com/documentation/idol/IDOL_12_0/MediaServer/Guides/html/English/Content/Getting_Started/Configure/_TRN_Set_up_PostgreSQL.htm
* How to Connect PostgreSQL Database (PostGIS) with Geoserver: https://www.youtube.com/watch?v=hpRJ8R9LB5E
* How to add WMS layer in OpenLayers from GeoServer?: https://gis.stackexchange.com/questions/58870/how-to-add-wms-layer-in-openlayers-from-geoserver
* How To Add WMS Layers From Geoserver To OpenLayers Map: https://www.youtube.com/watch?v=wCH7rvHFpCY
* How To Add and Style WFS (Vector) Layer From Geoserver To OpenLayers Map: https://www.youtube.com/watch?v=01TwGJ2dl_o
* GeoServer Security: https://docs.geoserver.org/latest/en/user/security/index.html
* Secure a layer in geoserver with password using OpenLayers: https://stackoverflow.com/questions/39019915/secure-a-layer-in-geoserver-with-password-using-openlayers
