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
* Simcoe County serves as my inspiration for this solution - I remember exploring this web solution after Shawn introduced it in class and I want to try to recreate an open source solution to fit this local governments needs and requirements.
 * Links to Simcoe County provided in class: https://github.com/county-of-simcoe-gis & https://opengis.simcoe.ca/
* After exploring Simcoe County's WebGIS solution - I believe that it can be utilized as a baseline idea for this problem statement. Searching for parcel boundaries, roads, parks and addresses, having aerial imagery as a basemap, and all are possible in this solution - which fits our clients criteria. 
* <img width="619" alt="image" src="https://github.com/aherstek/geom99exam/assets/146446987/a7644c17-1c15-4b6f-8ffd-146614202c43">

# Database / Data Tier - **PostgreSQL/PostGIS**

# Server / Middle Tier - **GeoServer and Google Cloud Platform**

## GeoServer

# Client / WebGIS Tier - **Open Layers**

