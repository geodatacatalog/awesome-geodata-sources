# Open geospatial data sources

> **Generated file — do not edit.** The source of truth is
> [`geodatacatalog/explorer/sources.yml`](geodatacatalog/explorer/sources.yml);
> regenerate with `geodatacatalog check` (probes every service) or `geodatacatalog md`.
>
> 145 sources · last check: 2026-08-08 · search them with
> `geodatacatalog search "forêt"` (FR/EN) and turn them into a config with
> `geodatacatalog add <source> <layer> -o catalog.yml`.

Status legend: ✅ working · ❌ failing · 🔐 credentials required · ❔ not checked yet
Fetch legend (`geodatacatalog check --fetch`): ✅ returned data · ∅ empty on test AOI · ❌ fetch error · 🔐 needs creds · ⏭ not fetchable

## 🛰️ Satellite imagery & Earth observation

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Earth Search (Element 84)](https://earth-search.aws.element84.com/v1) (`earth_search`) | Element 84 / AWS Open Data | STAC | global | — | ✅ 4087 ms | ✅ 25 feat | Sentinel-2 L2A COG, Sentinel-1 GRD, Landsat C2L2, NAIP, Copernicus DEM. API + Sentinel-2/DEM free & anonymous, but Sentinel-1 GRD (s3://sentinel-s1-l1c) and Landsat C2L2 (s3://usgs-landsat) are REQUESTER-PAYS — need AWS creds + requester-pays. For a free Sentinel-1 GRD in Europe use cop_dataspace instead. |
| [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/api/stac/v1) (`planetary_computer`) | Microsoft | STAC | global | — | ✅ 1526 ms | ✅ 47 feat | 126+ collections (incl. esa-worldcover 10 m land cover, cop-dem-glo-30 DEM). Assets are signed automatically (free, no account) when the planetary-computer package is installed — no credentials needed. |
| [Google Earth Engine (catalogue public)](https://earthengine-stac.storage.googleapis.com/catalog/catalog.json) (`gee`) | Google | STAC | global | oauth | ✅ 261 ms | 🔐 | Catalogue public Earth Engine (~900 datasets) publié en STAC statique. `discover` cherche par mot-clé dans la liste des datasets; le résultat est un id Earth Engine (ex. COPERNICUS/S2_SR_HARMONIZED) à utiliser avec la bibliothèque earthengine-api (ee.ImageCollection(id)) — pas de fetch direct via geodatacatalog. Accès données: compte Google + projet Cloud (gratuit usage non commercial), EARTHENGINE_PROJECT + GOOGLE_APPLICATION_CREDENTIALS dans .env. |
| [Copernicus Data Space STAC](https://stac.dataspace.copernicus.eu/v1) (`cop_dataspace`) | ESA / Copernicus | STAC | global | — | ✅ 1375 ms | ✅ 25 feat | Official ESA archive — Sentinel 1/2/3/5P. Data assets live on the eodata S3 store: set CDSE_S3_ACCESS_KEY / CDSE_S3_SECRET_KEY (free, from the S3 keys manager) before fetching, or reads fail with "AWS Access Key Id does not exist" / empty rasters. |
| [USGS LandsatLook](https://landsatlook.usgs.gov/stac-server) (`usgs_landsatlook`) | USGS | STAC | global | — | ✅ 3022 ms | ✅ 57 feat | Landsat Collection 2 |
| [NASA CMR-STAC (LPCLOUD)](https://cmr.earthdata.nasa.gov/cloudstac/LPCLOUD) (`nasa_cmr_lpcloud`) | NASA Earthdata | STAC | global | — | ✅ 3113 ms | ✅ 100 feat | HLS, ASTER, SRTM, NASADEM... Reading the data assets needs a free NASA Earthdata login (set up ~/.netrc for urs.earthdata.nasa.gov); without it the COGs come back as a login redirect and the fetch fails. |
| [GEODES (CNES)](https://geodes-portal.cnes.fr/api/stac) (`geodes`) | CNES | STAC | france | api_key | ✅ 3813 ms | 🔐 | THEIA products (OSO land cover, LAI, reflectances), Pléiades, SPOT. STAC 1.0.0-beta.2 — pystac_client get_collections() raises STACTypeError, so listing falls back to the raw /collections document. Item search/fetch needs GEODES_API_KEY. |
| [Sentinel Hub — indices & bandes spectrales (CDSE)](https://sh.dataspace.copernicus.eu/api/v1/process) (`sentinelhub`) | Copernicus Data Space / Sentinel Hub | REST | global | api_key | ✅ 247 ms | 🔐 | THE source for named spectral INDICES — any spyndex Sentinel-2 index (NDVI, NDMI, NBR, NDWI, EVI, SAVI...) — and single BANDS (B01-B12), computed on the fly from Sentinel-2 L2A (SCL cloud-masked) via the geodata_sentinelhubapi.py plugin. productType = the index/band name; prefix it with d (dNBR, dBAI, dNDVI...) for the CHANGE between the two ends of the datetime range (index[date2] - index[date1], e.g. burn severity). STAC collections only expose RAW bands, not named indices — use THIS for an index. Needs CDSE_CLIENT_ID/CDSE_CLIENT_SECRET. |
| [Digital Earth Africa](https://explorer.digitalearth.africa/stac) (`dea_africa`) | Digital Earth Africa | STAC | africa | — | ❌ 200 but no STAC JSON | ✅ 12 feat |  |
| [Digital Earth Australia](https://explorer.dea.ga.gov.au/stac) (`dea_australia`) | Geoscience Australia | STAC | australia | — | ✅ 902 ms | ✅ 21 feat |  |
| [CEDA STAC (UK)](https://api.stac.ceda.ac.uk) (`ceda_stac`) | CEDA | STAC | global | — | ✅ 237 ms | ∅ 0 STAC item(s) | Atmospheric and EO archives |
| [Capella Space Open Data (SAR)](https://capella-open-data.s3.us-west-2.amazonaws.com/stac/catalog.json) (`capella_opendata`) | Capella Space | REGISTRY | global | — | ✅ 617 ms | ⏭ | Static STAC catalog — SAR imagery samples |
| [NASA GIBS WMTS](https://gibs.earthdata.nasa.gov/wmts/epsg3857/best/wmts.cgi) (`nasa_gibs_wmts`) | NASA | WMTS | global | — | ✅ 8978 ms | ✅ raster 256 × 512 | Daily global imagery (MODIS, VIIRS...), also epsg4326/3413/3031. Hundreds of GIBS layers — this is the source behind many IntMap-style overlays: night lights / lumieres nocturnes (VIIRS Black Marble Night Lights), true-colour daily satellite / imagerie couleur naturelle, cloud fraction / couverture nuageuse, cloud-top temperature, water vapour / vapeur d'eau, land surface temperature day & night / temperature de surface, sea-surface temperature (SST) / temperature de surface de la mer et son anomalie, chlorophyll-a ocean colour / chlorophylle, aerosol optical depth & UV aerosol index / aerosols, snow & ice cover / neige et glace, sea-ice concentration / concentration de glace de mer, vegetation index NDVI / indice de vegetation. |
| [NASA GIBS WMS](https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi) (`nasa_gibs_wms`) | NASA | WMS | global | — | ✅ 328 ms | ❌ ServiceException: msWMSApplyTime: WMS se | GIBS WMS (GetMap on a bbox). Same catalogue as the WMTS tiles, handy when you need a georeferenced image rather than tiles: VIIRS/MODIS thermal anomalies / anomalies thermiques (active-fire overlays — see also nasa_firms for the point CSV), AMSRU2 sea-ice concentration / concentration de glace de mer, sea-surface temperature / temperature de surface de la mer, aerosol / aerosols, and night lights / lumieres nocturnes. Pass an explicit per-layer TIME= (the server default is a single stale date, so a bare request often returns a blank or out-of-range image). |
| [Maxar Open Data (imagerie catastrophes)](https://maxar-opendata.s3.amazonaws.com/events/catalog.json) (`maxar_open_data`) | Maxar | REGISTRY | global | — | ✅ 635 ms | ⏭ | High-resolution imagery for disaster events (CC-BY-NC), as a STATIC STAC catalog. Browse with pystac; not a STAC API (no /search endpoint), so list it, do not drill it. |
| [ESA WorldCover (couverture terrestre 10 m)](https://services.terrascope.be/wms/v2) (`esa_worldcover`) | ESA / VITO Terrascope | WMS | global | — | ❌ ClientOSError: [Errno 54] Connection reset by peer | ❔ | Global 10 m land cover (2020 and 2021 maps, Sentinel-1/2). Same data is on Planetary Computer as STAC collection esa-worldcover (COGs). |
| [Esri World Imagery Wayback (imagerie historique)](https://wayback.maptiles.arcgis.com/arcgis/rest/services/World_Imagery/WMTS/1.0.0/WMTSCapabilities.xml) (`esri_wayback`) | Esri | WMTS | global | — | ✅ 600 ms | ❌ AttributeError: 'NoneType' object has no | Every historical release of the Esri World Imagery basemap (2014→today) as WMTS layers (WB_<year>_R<nn>) — visual change detection over a decade. Free for non-commercial use. |

## 🗺️ Basemaps & general purpose

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [IGN Géoplateforme WMS raster](https://data.geopf.fr/wms-r/wms) (`ign_wms_raster`) | IGN | WMS | france | — | ✅ 130 ms | ✅ raster 64 × 64 | Orthophotos, Plan IGN, altitude... |
| [IGN Géoplateforme WMS vecteur](https://data.geopf.fr/wms-v/ows) (`ign_wms_vector`) | IGN | WMS | france | — | ✅ 255 ms | ✅ raster 146 × 105 |  |
| [IGN Géoplateforme WMTS](https://data.geopf.fr/wmts) (`ign_wmts`) | IGN | WMTS | france | — | ✅ 122 ms | ✅ raster 512 × 512 | Orthophotos, Plan IGN v2, BD Forêt v2, Corine Land Cover |
| [SHOM (littoral & marine)](https://services.data.shom.fr/INSPIRE/wms/r) (`shom`) | SHOM | WMS | france | — | ✅ 1185 ms | ✅ raster 64 × 64 | Nautical charts, coastal data |
| [Open Maps For Europe](https://www.mapsforeurope.org/) (`open_maps_europe`) | EuroGeographics | REGISTRY | europe | — | ✅ 194 ms | ⏭ |  |
| [USGS The National Map (services)](https://apps.nationalmap.gov/services/) (`usgs_tnm`) | USGS | REGISTRY | usa | — | ✅ 105 ms | ⏭ |  |
| [Allemagne — BKG TopPlus Open](https://sgx.geodatenzentrum.de/wms_topplus_open) (`bkg_topplus`) | BKG (Allemagne) | WMS | germany | — | ✅ 362 ms | ✅ raster 87 × 106 |  |
| [Royaume-Uni — Ordnance Survey Maps API](https://api.os.uk/maps/raster/v1/wmts) (`os_uk`) | Ordnance Survey | WMTS | uk | api_key | 🔐 | 🔐 |  |
| [Pays-Bas — PDOK](https://service.pdok.nl/) (`pdok_nl`) | PDOK | REGISTRY | netherlands | — | ❌ HTTP 404 | ❔ | Very rich (WMS/WFS/WMTS/OGC API per dataset) |
| [Espagne — IGN base](https://www.ign.es/wms-inspire/ign-base) (`ign_es`) | IGN España | WMS | spain | — | ✅ 67 ms | ✅ raster 87 × 89 |  |
| [Suisse — swisstopo geo.admin](https://wms.geo.admin.ch/) (`swisstopo`) | swisstopo | WMS | switzerland | — | ✅ 279 ms | ✅ raster 64 × 80 |  |
| [Belgique — Geopunt Vlaanderen](https://geo.api.vlaanderen.be/) (`vlaanderen_be`) | Vlaanderen | REGISTRY | belgium | — | ✅ 173 ms | ⏭ |  |
| [Canada — GéoBase CanVec](https://maps.geogratis.gc.ca/wms/canvec_en) (`nrcan_canvec`) | NRCan | WMS | canada | — | ✅ 803 ms | ❌ RuntimeError: Failed after 4 attempts: H |  |
| [Norvège — Kartverket topo](https://wms.geonorge.no/skwms1/wms.topo) (`geonorge_no`) | Kartverket | WMS | norway | — | ✅ 338 ms | ✅ raster 64 × 64 |  |
| [Nouvelle-Zélande — LINZ Data Service](https://data.linz.govt.nz/) (`linz_nz`) | LINZ | REGISTRY | new-zealand | api_key | ✅ 1800 ms | ⏭ | LINZ Data Service — WMS/WMTS/WFS with free API key |
| [HOT Export Tool (OSM humanitaire)](https://export.hotosm.org/) (`hotosm_export`) | Humanitarian OSM Team | REGISTRY | global | — | ✅ 577 ms | ⏭ |  |
| [Natural Earth — Rivers & Lake Centerlines](https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson) (`natural_earth_rivers`) | Natural Earth | REST | global | — | ✅ 306 ms | ✅ 1455 feat | Global river/fleuve POLYLINES (one named line per major river — the Nile, Amazon…), plus lake centerlines. Static GeoJSON (the whole file is fetched, then clipped to the AOI client-side), EPSG:4326. Use 10m for detail/names, 50m or 110m for a light world overview. No server-side bbox filter. |
| [OSM Overpass API](https://overpass-api.de/api/interpreter) (`overpass`) | OpenStreetMap | REST | global | — | ✅ 215 ms | ✅ 49 feat | OSM vector features via Overpass QL (POST) → points/lines/polygons. The curated list is a STARTER set — productType is ANY OSM tag filter: a key (highway), a key=value (amenity=restaurant), or several separated by , / \| (any-of, e.g. amenity=school,amenity=university). A tag filter alone matches EVERY feature with that tag in the AOI — append ;name=<text> to match a SINGLE named feature instead (e.g. waterway=river;name=Nile). Query per AOI/region (a planet-wide request times out). See the OSM wiki Map Features for every available key/value. |
| [OSM Nominatim (géocodage)](https://nominatim.openstreetmap.org) (`nominatim`) | OpenStreetMap | REST | global | — | ✅ 224 ms | ❌ HTTPError: 403 Client Error: Forbidden f |  |
| [OSM standard tiles](https://tile.openstreetmap.org/{z}/{x}/{y}.png) (`osm_tiles`) | OpenStreetMap | XYZ | global | — | ✅ 75 ms | ⏭ |  |
| [CARTO basemaps (dark / light / voyager)](https://a.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png) (`carto_basemaps`) | CARTO | XYZ | global | — | ✅ 83 ms | ⏭ | Free global raster basemaps (© CARTO © OpenStreetMap). The endpoint is the dark night basemap — a fond de carte sombre pour la nuit. Swap the style path for its siblings: light_all (positron clair), dark_all (dark nuit sombre), voyager, dark_nolabels, light_nolabels. XYZ raster tiles, so add them with add_xyz_tile_layer (they are NOT MapLibre style JSONs — do not pass them to change_basemap). |
| [Overture Maps (vector tiles)](https://overturemaps.org/) (`overture_maps`) | Overture Maps Foundation | REST | global | — | ✅ 237 ms | ✅ 18193 feat | Global harmonised features (OSM + Meta + Microsoft + Esri + Foursquare). DEFAULT plugin `overture_tiles.py` reads the official Overture PMTiles (range-read of the few covering tiles) → FAST on a live bbox, at the cost of tile generalisation/clipping and a reduced attribute set (needs pmtiles + mapbox-vector-tile + mercantile). For EXACT geometry / all columns / bulk extraction over large areas, switch the provider to `plugin: overture.py` (GeoParquet over S3 via DuckDB — slower on a live bbox, transportation/buildings span hundreds of files). `pip install geodatacatalog[overture]`. |
| [Natural Earth (vecteurs monde)](https://www.naturalearthdata.com/downloads/) (`natural_earth`) | Natural Earth | REGISTRY | global | — | ✅ 626 ms | ⏭ |  |
| [IGN Géoplateforme catalogue (CSW)](https://data.geopf.fr/csw) (`geoplateforme_csw`) | IGN (Géoplateforme) | CSW | france | — | ✅ 69 ms | ⏭ | Metadata catalogue of the French Géoplateforme (IGN national platform) |
| [Historical country borders (aourednik)](https://raw.githubusercontent.com/aourednik/historical-basemaps/master/geojson) (`historical_basemaps`) | André Ourednik (open data) | REST | global | — | ✅ 428 ms | ✅ 270 feat | World political boundaries reconstructed for many historical years (BC 123000 to 1994), one GeoJSON file per year (world_<year>.geojson). Whole world per file, clip client-side. |

## 🏛️ Administrative, cadastre & urban

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [IGN Géoplateforme WFS](https://data.geopf.fr/wfs/ows) (`ign_wfs`) | IGN | WFS | france | — | ✅ 441 ms | ✅ 8 feat | BD TOPO, cadastre, RPG, ZNIEFF/Natura 2000, BD Forêt... (hundreds of layers) |
| [OpenRouteService (routage & isochrones)](https://api.openrouteservice.org) (`openrouteservice`) | HeiGIT / openrouteservice | REST | global | api_key | ✅ 184 ms | 🔐 | Routing, isochrones and matrices over OSM. Free API key required (Authorization header). profile = driving-car\|foot-walking\|cycling-regular... |
| [Eurostat GISCO (NUTS, LAU, pays)](https://gisco-services.ec.europa.eu/distribution/v2) (`eurostat_gisco`) | Eurostat | REST | europe | — | ✅ 178 ms | ✅ 2010 feat | EU statistical boundaries as static GeoJSON files (whole-Europe download per file, no bbox filter); statistics themselves are on the separate Eurostat dissemination API (ec.europa.eu/eurostat/api/dissemination). |
| [geoBoundaries (limites administratives monde)](https://www.geoboundaries.org/api/current) (`geoboundaries`) | William & Mary geoLab | REST | global | — | ✅ 438 ms | ❌ HTTPError: 404 Client Error: Not Found f | Open database of administrative boundaries for every country. The API returns metadata JSON with a gjDownloadURL (GeoJSON on GitHub) — no bbox filter, download the level you need client-side. |

## ⛰️ Elevation & terrain

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [GEE Community Catalog (datasets communautaires Earth Engine)](https://gee-community-catalog.org) (`gee_community`) | Samapriya Roy / communauté (awesome-gee-community-datasets) | REGISTRY | global | oauth | ✅ 403 ms | ⏭ | Catalogue communautaire Earth Engine (~4400 datasets hébergés dans EE sous projects/sat-io/..., ex. canopée Meta/WRI 1 m — gee-community-catalog.org/projects/meta_trees), complémentaire du catalogue public Google (source `gee`) qui ne les référence pas. `discover` cherche par mot-clé dans la liste JSON du dépôt awesome-gee-community-datasets; le résultat est un id Earth Engine (ex. projects/sat-io/open-datasets/facebook/meta-canopy-height) à utiliser avec la bibliothèque earthengine-api — pas de fetch direct via geodatacatalog. Mêmes credentials que la source gee. |
| [IGN Géoplateforme altimétrie (REST)](https://data.geopf.fr/altimetrie) (`ign_alti`) | IGN | REST | france | — | ✅ 182 ms | ✅ 1 feat | RGE Alti point elevation API (the old WCS endpoint was retired). Point query — samples the bbox centre (lon/lat), not a bbox. |
| [EMODnet Bathymetry (WMS)](https://ows.emodnet-bathymetry.eu/wms) (`emodnet_bathymetry`) | EMODnet | WMS | europe | — | ✅ 232 ms | ✅ raster 146 × 107 | European bathymetry DTM (also /wfs and /wcs on the same host) |
| [EMODnet Bathymetry (WCS)](https://ows.emodnet-bathymetry.eu/wcs) (`emodnet_bathymetry_wcs`) | EMODnet | WCS | europe | — | ✅ 130 ms | ❌ HTTPError: 404 Client Error: Not Found f |  |
| [GEBCO global bathymetry](https://wms.gebco.net/mapserv) (`gebco`) | GEBCO / BODC | WMS | global | — | ✅ 173 ms | ✅ raster 146 × 107 |  |
| [USGS 3DEP Elevation (WCS)](https://elevation.nationalmap.gov/arcgis/services/3DEPElevation/ImageServer/WCSServer) (`usgs_3dep`) | USGS | WCS | usa | — | ✅ 1679 ms | ❌ HTTPError: 404 Client Error: Not Found f |  |
| [OpenTopography Global DEM API](https://portal.opentopography.org/API/globaldem) (`opentopography`) | OpenTopography | REST | global | api_key | ❌ HTTP 400 | ❔ | SRTM, Copernicus DEM, ALOS... (free API key) |
| [Australie — Geoscience Australia](https://services.ga.gov.au/) (`ga_australia`) | Geoscience Australia | REGISTRY | australia | — | ✅ 1162 ms | ⏭ | Geoscience Australia services portal (WMS/WFS per dataset) |
| [USGS Elevation Point Query (EPQS)](https://epqs.nationalmap.gov/v1) (`usgs_epqs`) | USGS | REST | usa | — | ✅ 1367 ms | ✅ 1 feat | Point query: samples the bbox centre, returns one feature with the 3DEP elevation `value` (US and territories). For DEM rasters use the 3DEP ImageServer or opentopography. |
| [AWS Terrain Tiles (global DEM, Terrarium)](https://s3.amazonaws.com/elevation-tiles-prod/terrarium/{z}/{x}/{y}.png) (`aws_terrain_tiles`) | AWS Open Data / Mapzen | XYZ | global | — | ✅ 342 ms | ⏭ | Global elevation as Terrarium-encoded raster tiles (RGB → metres), assembled from SRTM, ETOPO1, GMTED, NED and others. Free & anonymous. Decode height = (R*256 + G + B/256) - 32768; also served as normal, geotiff and skadi formats. |

## 🌍 Land cover & land use

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [EEA Discomap (ArcGIS REST)](https://image.discomap.eea.europa.eu/arcgis/rest/services) (`eea_discomap`) | European Environment Agency | REGISTRY | europe | — | ✅ 1215 ms | ⏭ |  |
| [Corine Land Cover 2018 (EEA)](https://image.discomap.eea.europa.eu/arcgis/services/Corine/CLC2018_WM/MapServer/WMSServer) (`eea_clc2018`) | European Environment Agency | WMS | europe | — | ✅ 1266 ms | ✅ raster 146 × 105 | ArcGIS MapServer WMS, SCALE-TIERED: each numbered layer (1..13) is visible only at one zoom level, so a single numbered layer renders transparent at other scales. Request them ALL together — layer "1,2,3,4,5,7,8,9,10,11,12,13" (comma-separated GetMap) — to display Corine Land Cover at every zoom. Do NOT propose a single numbered layer. |
| [EMODnet Seabed Habitats (EUSeaMap)](https://ows.emodnet-seabedhabitats.eu/geoserver/emodnet_open/wms) (`emodnet_habitats`) | EMODnet | WMS | europe | — | ✅ 243 ms | ✅ raster 146 × 107 |  |
| [OpenLandMap STAC](https://s3.eu-central-1.wasabisys.com/stac/openlandmap/catalog.json) (`openlandmap`) | OpenGeoHub | REGISTRY | global | — | ✅ 243 ms | ⏭ |  |
| [MRLC NLCD (land cover USA)](https://www.mrlc.gov/geoserver/ows) (`mrlc_nlcd`) | MRLC | WMS | usa | — | ✅ 754 ms | ❌ ServiceException: java.io.IOException: F |  |
| [EEA SDI catalogue (CSW)](https://sdi.eea.europa.eu/catalogue/srv/eng/csw) (`eea_sdi`) | European Environment Agency | CSW | europe | — | ✅ 268 ms | ⏭ | Searchable EU environmental metadata catalogue (datasets + services) |
| [Global Forest Watch Data API](https://data-api.globalforestwatch.org) (`gfw_data_api`) | WRI / Global Forest Watch | REST | global | — | ✅ 791 ms | ∅ 0 feature(s) | Deforestation and forest change: tree cover loss, GLAD and RADD alerts, primary forest, key biodiversity areas. The dataset list is open; queries/downloads of some datasets need a free API key. |
| [RESOLVE Ecoregions 2017 (WWF terrestrial biomes)](https://services.arcgis.com/P3ePLMYs2RVChkJx/arcgis/rest/services/Terrestrial_Ecoregions_World/FeatureServer) (`resolve_ecoregions`) | RESOLVE / WWF | REST | global | — | ✅ 493 ms | ❌ JSONDecodeError: Expecting value: line 1 | RESOLVE Ecoregions 2017 (846 ecoregions, 14 biomes) — the standard biogeographic units. ArcGIS FeatureServer: query 0/query?where=1=1&outFields=*&f=geojson (add geometry/geometryType=esriGeometryEnvelope &inSR=4326 for a bbox). Original data also as a shapefile ZIP from ecoregions.appspot.com. |

## 🌲 Forest

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Copernicus EFFIS (feux de forêt)](https://maps.effis.emergency.copernicus.eu/effis) (`effis`) | Copernicus Emergency | WMS | europe | — | ✅ 164 ms | ✅ raster 146 × 105 | Fire info — WMS layers use prefixed codes, not plain words: fire danger indices are mf010.fwi / mf010.ffmc / mf010.dmc / mf010.dc / mf010.isi / mf010.bui (NOT bare "fwi"), hs (active fire hot spots: modis/viirs/noaa), ba (burnt areas), fuel_map. Grep these codes, not "fire". GetMap REQUIRES an explicit STYLES= parameter, even empty (MapServer 8). TIME-dimensioned layers (fire danger indices) silently render BLANK without an explicit TIME=<date>: the server default is stale (2021-01-01) — always pass a recent date. EXCEPTION — HOTSPOTS (all.hs, viirs.hs, modis.hs...): they declare a time dimension but an explicit TIME= renders EMPTY everywhere; request them WITHOUT TIME to get the current detections. BURNT AREAS: this is a raster-only WMS — the .poly (polygon) variants render BLANK through GetMap (vectors need WFS/download, not exposed here), so for a visible map or a fetch use the RASTER layer modis.ba.<year> (e.g. modis.ba.2024) or modis.ba.point.<year>, NEVER modis.ba.poly.* . effis.nrt.ba is BROKEN server-side (checked 2026-07-13): the group includes effis.nrt.ba.point whose GetMap answers HTTP 200 text/xml ServiceException "msPostGISLayerWhichShapes(): Query error" — for near-real-time burnt areas use nrt.ba.today / nrt.ba.week on the gwis endpoint (source id: gwis) instead. |

## 🟤 Soils

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [ISRIC SoilGrids — sand](https://maps.isric.org/mapserv?map=/map/sand.map) (`soilgrids_sand`) | ISRIC | WCS | global | — | ✅ 6751 ms | ✅ raster 128 × 128 | Other properties use the same pattern: clay, silt, soc, phh2o, nitrogen, bdod, cec... |
| [ISRIC SoilGrids — clay](https://maps.isric.org/mapserv?map=/map/clay.map) (`soilgrids_clay`) | ISRIC | WCS | global | — | ✅ 3926 ms | ✅ raster 128 × 128 |  |
| [ISRIC SoilGrids — organic carbon](https://maps.isric.org/mapserv?map=/map/soc.map) (`soilgrids_soc`) | ISRIC | WCS | global | — | ✅ 3548 ms | ✅ raster 128 × 128 |  |
| [ISRIC SoilGrids REST API](https://rest.isric.org/soilgrids/v2.0) (`soilgrids_rest`) | ISRIC | REST | global | — | ✅ 1183 ms | ✅ 1 feat | Point query: samples the bbox centre and returns a single GeoJSON Feature (soil properties in properties.layers). Set property/depth/value as needed (clay, sand, soc, phh2o, nitrogen...). |
| [USDA Soil Data Access](https://sdmdataaccess.sc.egov.usda.gov) (`usda_sda`) | USDA | REST | usa | — | ✅ 609 ms | ⏭ |  |

## 💧 Inland water

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Marine Regions (VLIZ) WFS](https://geo.vliz.be/geoserver/MarineRegions/wfs) (`marine_regions`) | VLIZ / Marine Regions | WFS | global | — | ✅ 137 ms | ✅ 3 feat | Named sea/ocean polygons — Global Oceans & Seas (MarineRegions:goas), IHO Sea Areas (MarineRegions:iho). Also the maritime-boundary layers: Exclusive Economic Zones / ZEE (MarineRegions:eez, MarineRegions:eez_boundaries) and the 12-nautical-mile territorial seas / limites des 12 milles marins (MarineRegions:eez_12nm, plus eez_24nm contiguous zone). Ideal AOI source for marine areas (e.g. Mediterranean). |
| [Géorisques (risques naturels)](https://www.georisques.gouv.fr/services) (`brgm_risques`) | BRGM / Géorisques | WMS | france | — | ✅ 2079 ms | ✅ raster 146 × 105 | Risques naturels FR (inondation, mouvements de terrain, retrait-gonflement des argiles, séismes, cavités...). WMS Géorisques — remplace l'ancien geoservices.brgm.fr/risques (fermé). Couches listées en live via GetCapabilities. |
| [SANDRE Eau France (géo)](https://services.sandre.eaufrance.fr/geo/sandre) (`sandre`) | SANDRE / OFB | WFS | france | — | ✅ 1063 ms | ✅ 30 feat | BD Topage — water bodies, watercourses |
| [SANDRE masses d'eau](https://services.sandre.eaufrance.fr/geo/mdo) (`sandre_mdo`) | SANDRE / OFB | WFS | france | — | ✅ 254 ms | ✅ 1 feat |  |
| [Hub'Eau (APIs eau France)](https://hubeau.eaufrance.fr/api) (`hubeau`) | BRGM / OFB | REST | france | — | ✅ 124 ms | ✅ 75 feat | Hydrometry, piezometry, water quality, fish... (station endpoints support bbox + format=geojson) |
| [EPA environmental services](https://geopub.epa.gov/arcgis/rest/services) (`epa_geoservices`) | US EPA | REGISTRY | usa | — | ✅ 596 ms | ⏭ |  |
| [Royaume-Uni — DEFRA / Environment Agency](https://environment.data.gov.uk/spatialdata/) (`defra_uk`) | DEFRA | REGISTRY | uk | — | ❌ HTTP 404 | ❔ |  |
| [USACE National Inventory of Dams (NID)](https://nid.sec.usace.army.mil/) (`usace_nid`) | US Army Corps of Engineers | REGISTRY | usa | — | ✅ 747 ms | ⏭ | Inventory of US dams (location, hazard class, height, purpose). Bulk download (CSV/GeoJSON) from the portal; the JSON API has no documented open list route. |
| [WRI Aqueduct 4.0 (risque eau par bassin)](https://livingatlas.esri.in/server/rest/services/Aqueduct_Water_Risk/FeatureServer) (`wri_aqueduct`) | World Resources Institute (hébergement Esri Living Atlas) | REST | global | — | ✅ 11776 ms | ∅ 0 feature(s) | WRI Aqueduct 4.0 water-risk indicators (baseline water stress, drought risk, riverine/coastal flood risk...) per HydroBASINS sub-basin, served as an ArcGIS FeatureServer (f=geojson). |

## 🌊 Oceans & marine

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Copernicus Marine WMTS](https://wmts.marine.copernicus.eu/teroWmts) (`cmems_wmts`) | Copernicus Marine | WMTS | global | — | ✅ 305 ms | ✅ raster 512 × 512 | Visualisation tiles; full data via data.marine.copernicus.eu (account) |
| [EMODnet Biology](https://geo.vliz.be/geoserver/Emodnetbio/wfs) (`emodnet_biology`) | EMODnet / VLIZ | WFS | europe | — | ✅ 126 ms | ∅ 0 feature(s) |  |
| [EMODnet Human Activities](https://ows.emodnet-humanactivities.eu/wfs) (`emodnet_human`) | EMODnet | WFS | europe | — | ✅ 239 ms | ∅ 0 feature(s) | Human uses of the sea (WFS vectors): port vessels (portvessels, portvesselsbytype), fishing effort by vessel type, pipelines, wind farms, aggregates... AIS vessel/route density grids are the raster product on the WMS — see emodnet_vessel_density. |
| [EMODnet Vessel Density (trafic maritime AIS)](https://ows.emodnet-humanactivities.eu/wms) (`emodnet_vessel_density`) | EMODnet Human Activities | WMS | europe | — | ✅ 260 ms | ✅ raster 146 × 107 | Monthly AIS-derived vessel & route density grids for all European seas (open, no auth). Raster layers VesselDensity/RouteDensity and per-ship-type vesseldensity_NN (00=all, then fishing, cargo, tanker, passenger...), each with _avg and _seasonal variants. |
| [NOAA ERDDAP (CoastWatch)](https://coastwatch.pfeg.noaa.gov/erddap/index.html) (`noaa_erddap`) | NOAA | REGISTRY | global | — | ✅ 933 ms | ⏭ | Huge oceanographic data server (griddap/tabledap) |
| [OBIS (biodiversité marine)](https://api.obis.org/v3) (`obis`) | UNESCO / IOC | REST | global | — | ✅ 1038 ms | ✅ 200 feat |  |
| [NOAA nowCOAST](https://nowcoast.noaa.gov/) (`noaa_nowcoast`) | NOAA | REGISTRY | usa | — | ✅ 231 ms | ⏭ |  |
| [Ifremer Sextant (CSW)](https://sextant.ifremer.fr/geonetwork/srv/fre/csw) (`sextant`) | Ifremer | CSW | europe | — | ✅ 267 ms | ⏭ | Marine and coastal metadata catalogue (Ifremer Sextant) |
| [Digitraffic Marine (trafic maritime AIS)](https://meri.digitraffic.fi/api) (`digitraffic_marine`) | Fintraffic | REST | finland | — | ✅ 182 ms | ❌ HTTPError: 400 Client Error: Bad Request | Open AIS vessel traffic in Finnish waters only (Fintraffic) — not Europe-wide. Returns a GeoJSON FeatureCollection (geometry, not flat lon/lat), so it needs GeoJSON parsing. The server requires an Accept-Encoding gzip header. |
| [Global Fishing Watch API (trafic & pêche AIS)](https://gateway.api.globalfishingwatch.org/v3) (`global_fishing_watch`) | Global Fishing Watch | REST | global | api_key | 🔐 | 🔐 | Global AIS-based vessel tracking and apparent fishing effort. Free API token required (Bearer header; register at globalfishingwatch.org). 4wings/report returns gridded activity and events return point/track features — needs a plugin for clean geo parsing. |
| [MarineCadastre AIS (USA)](https://marinecadastre.gov/ais/) (`marine_cadastre`) | NOAA / BOEM | REGISTRY | usa | — | ✅ 1000 ms | ⏭ | US AIS vessel-traffic archive (NOAA/BOEM): yearly/daily AIS point tracks and vessel-transit-count rasters for US waters. Bulk download (CSV/GeoTIFF), not a live API. |
| [TeleGeography Submarine Cable Map](https://www.submarinecablemap.com/api/v3) (`submarine_cables`) | TeleGeography | REST | global | — | ✅ 483 ms | ✅ 718 feat | Open GeoJSON, whole world in one file — the static endpoint ignores bbox/params, so it returns all features (clip client-side). Cables are lines, landing points are points. |
| [IMF PortWatch (trafic portuaire & perturbations)](https://services9.arcgis.com/weJ1QsnbMYJlCHdG/arcgis/rest/services) (`imf_portwatch`) | IMF / Oxford | REST | global | — | ✅ 268 ms | ∅ 0 feature(s) | portwatch.imf.org open data (ArcGIS Online, f=geojson): AIS-derived port activity and trade disruption monitoring by the IMF and Oxford. |
| [OpenSeaMap nautical seamarks (tiles)](https://tiles.openseamap.org/seamark/{z}/{x}/{y}.png) (`openseamap`) | OpenSeaMap / OpenStreetMap | XYZ | global | — | ✅ 214 ms | ⏭ | Nautical seamark overlay tiles (buoys, lights, harbours, depth marks) from OSM. Transparent PNG overlay; underlying seamark features are queryable via Overpass (seamark:* tags). |

## 🔥 Fire

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Copernicus GWIS (feux mondiaux)](https://maps.effis.emergency.copernicus.eu/gwis) (`gwis`) | Copernicus Emergency | WMS | global | — | ✅ 154 ms | ✅ raster 146 × 105 | Global fire info (MapServer WMS, same family as effis; GetMap REQUIRES an explicit STYLES= parameter, even empty, and TIME-dimensioned layers render BLANK without an explicit recent TIME=<date>; hotspots hs layers are the exception: request them WITHOUT TIME). NRT BURNT AREAS are SCALE-DEPENDENT: the bare nrt.ba group only draws at world zooms (z<=2), .point variants only at low zooms, .poly variants only from ~z9 — a blank tile at one zoom does NOT mean "no data". For a visible map use the date-windowed groups nrt.ba.today / nrt.ba.week / nrt.ba.month / nrt.ba.season (point+poly combined with scale rules, what the official EFFIS viewer uses), no TIME needed; for composites use mcd64a1.* (e.g. mcd64a1.annual_composite.c12, mcd64a1.fire_frequency). These windowed groups render SLOWLY on first hit (~15-30 s per tile, then instant from the server cache): tell the user to leave the map still for ~30 s — browser "GET aborted" on their tiles just means MapLibre cancelled a pending tile on pan/zoom, not a server failure. gwis.globfire.finalperim is BROKEN server-side (checked 2026-07-13): every GetMap answers HTTP 200 text/xml ServiceException "msPostGISLayerWhichShapes(): Query error" regardless of BBOX/TIME/CRS — use nrt.ba.today/.week or the mcd64a1.* composites for burnt areas instead. |
| [NASA FIRMS (feux actifs)](https://firms.modaps.eosdis.nasa.gov/api) (`nasa_firms`) | NASA | REST | global | api_key | ✅ 931 ms | 🔐 | MODIS/VIIRS near-real-time active fires / feux actifs, i.e. thermal anomalies / anomalies thermiques (the fire-hotspot layer). Free MAP_KEY required (in the URL path). Returns CSV with latitude/longitude — needs CSV parsing. For a ready-made raster overlay of the same product see nasa_gibs_wms. |
| [NASA EONET (événements naturels en direct)](https://eonet.gsfc.nasa.gov/api/v3) (`nasa_eonet`) | NASA | REST | global | — | ✅ 449 ms | ❌ HTTPError: 404 Client Error: Not Found f | Earth Observatory Natural Event Tracker: curated live + historical natural events aggregated from GDACS, InciWeb, USGS... bbox is upper-left/lower-right (lonmin,latmax,lonmax,latmin). |

## ⚠️ Hazards & disasters

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [GDACS (alertes catastrophes)](https://www.gdacs.org/gdacsapi) (`gdacs`) | UN / EC JRC | REST | global | — | ✅ 451 ms | ✅ 233 feat |  |
| [NOAA NWS API (météo & alertes)](https://api.weather.gov) (`nws_api`) | NOAA | REST | usa | — | ✅ 252 ms | ❌ HTTPError: 400 Client Error: Bad Request |  |
| [USGS Earthquake API](https://earthquake.usgs.gov/fdsnws/event/1) (`usgs_earthquake`) | USGS | REST | global | — | ✅ 75 ms | ✅ 320 feat | fdsnws uses min/maxlongitude & min/maxlatitude (not a bbox param) and returns GeoJSON; the REST layer now parses FeatureCollection geometry. Default test bbox is central California (active). |
| [ReliefWeb API (humanitaire)](https://api.reliefweb.int/v2) (`reliefweb`) | UN OCHA | REST | global | — | ❌ HTTP 403 | ❔ |  |
| [GDELT 2.0 (événements & actualités géolocalisés)](https://api.gdeltproject.org/api/v2) (`gdelt`) | GDELT Project | REST | global | — | ✅ 14403 ms | ❌ HTTPError: 429 Client Error: Too Many Re | World news and events, updated every 15 min; rate-limited to about one request per 5 s. The companion geo/geo endpoint returns GeoJSON of place mentions. |
| [FEMA OpenFEMA (catastrophes USA)](https://www.fema.gov/api/open) (`fema_openfema`) | US FEMA | REST | usa | — | ✅ 303 ms | ∅ 0 feature(s) | US disaster and hazard data (declarations, NFIP claims, mitigation). Mostly tabular by FIPS; flood-zone POLYGONS are the separate NFHL ArcGIS/WMS service at hazards.fema.gov. Uses $-prefixed OData params. |
| [ACLED (conflits & manifestations géolocalisés)](https://api.acleddata.com) (`acled`) | ACLED | REST | global | api_key | ❌ ClientConnectorDNSError: Cannot connect to host api.acleddata.com:443 ssl:default [nodename nor servname provided, or not known] | ❔ | Armed Conflict Location & Event Data: geolocated political violence and protest events, weekly updates. Free registration; set ACLED_KEY and ACLED_EMAIL in .env (sent as key/email query params). |
| [Smithsonian Global Volcanism Program (GVP) WFS](https://webservices.volcano.si.edu/geoserver/GVP-VOTW/ows) (`gvp_volcanoes`) | Smithsonian Institution | WFS | global | — | ✅ 341 ms | ∅ 0 feature(s) | Volcanoes of the World database — all 1,300+ Holocene volcanoes as points (GVP-VOTW:Smithsonian_VOTW_Holocene_Volcanoes), plus Pleistocene volcanoes and Holocene eruptions. GeoServer WFS, GetFeature returns GeoJSON. GVP-VOTW:E3WebApp_Emissions is broken server-side (backing DB view vrf.Emission missing, every GetFeature fails; checked 2026-07-17) — do not propose it. |
| [RainViewer precipitation radar (nowcast)](https://api.rainviewer.com/public) (`rainviewer`) | RainViewer | XYZ | global | — | ✅ 135 ms | ⏭ | Global live precipitation radar with ~2 h nowcast. weather-maps.json lists timestamped frames whose paths build XYZ tiles ({host}{path}/{size}/{z}/{x}/{y}/{color}/{options}.png). The XYZ layer resolves the manifest (frame nearest the requested datetime, else the newest) and mosaics the tiles into a raster; also drapeable client-side as live tiles. Frames are ephemeral — a fetched raster is a snapshot. |

## 🌦️ Weather & climate

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Copernicus Climate Data Store API](https://cds.climate.copernicus.eu/api) (`copernicus_cds`) | Copernicus / ECMWF | REST | global | api_key | ✅ 189 ms | 🔐 | ERA5, seasonal forecasts... (cdsapi client) |
| [ECMWF Open Data (prévisions)](https://data.ecmwf.int/forecasts/) (`ecmwf_opendata`) | ECMWF | REST | global | — | ✅ 766 ms | ❌ HTTPError: 404 Client Error: Not Found f |  |
| [NOAA NOMADS (GFS...)](https://nomads.ncep.noaa.gov/) (`noaa_nomads`) | NOAA | REGISTRY | global | — | ✅ 240 ms | ⏭ |  |
| [Open-Meteo (prévisions & archives météo)](https://api.open-meteo.com/v1) (`open_meteo`) | Open-Meteo | REST | global | — | ✅ 153 ms | ❌ JSONDecodeError: Expecting value: line 1 | Free weather API, no key: forecasts (16 days) and, on archive-api.open-meteo.com/v1/archive, historical reanalysis since 1940. Point-based (latitude/longitude params), returns JSON time-series arrays — no bbox and not per-record lon/lat, so registered for discovery; fetch it client-side or via a plugin. |
| [NOAA Space Weather Prediction Center](https://services.swpc.noaa.gov) (`noaa_swpc`) | NOAA SWPC | REST | global | — | ✅ 62 ms | ∅ 0 feature(s) | Live space-weather feeds (aurora forecast, Kp index, solar wind). The aurora grid is a positional [lon, lat, value] array, not flat lon/lat records, so RESTLayer will not geolocate it directly — register for discovery, parse in a plugin/client. |

## 🌡️ Climate

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Köppen-Geiger climate classification (GloH2O)](https://www.gloh2o.org/koppen/) (`koppen_geiger`) | GloH2O (Beck et al.) | REGISTRY | global | — | ✅ 554 ms | ⏭ | Global Köppen-Geiger climate classification maps (Beck et al. 2018 & 2023) as downloadable GeoTIFF rasters at 1 km to 1° resolution, for 1901-1930 through 1991-2020 and future scenarios. Portal/archive download (Zenodo/figshare), no live tile or query API — register for discovery, load the GeoTIFF locally. |

## 🐾 Biodiversity & ecology

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [INPN / MNHN biodiversité](https://odata-inpn.mnhn.fr) (`inpn_odata`) | MNHN | REST | france | — | ✅ 316 ms | ⏭ |  |
| [Natura 2000 (EEA bio Discomap)](https://bio.discomap.eea.europa.eu/arcgis/rest/services) (`eea_natura2000`) | European Environment Agency | REGISTRY | europe | — | ✅ 813 ms | ⏭ |  |
| [GBIF (occurrences d'espèces)](https://api.gbif.org/v1) (`gbif`) | GBIF | REST | global | — | ✅ 195 ms | ⏭ | Species occurrences worldwide. productType = scientific name (e.g. Gypaetus barbatus) |
| [iNaturalist API](https://api.inaturalist.org/v1) (`inaturalist`) | iNaturalist | REST | global | — | ✅ 747 ms | ✅ 30 feat | Observations are returned with a per-record `geojson` Point (no flat lon/lat); the bbox uses nelat/nelng/swlat/swlng. taxa/places are queried differently (q=/lat,lng). |
| [Protected Planet (WDPA)](https://www.protectedplanet.net/) (`protected_planet`) | UNEP-WCMC | REGISTRY | global | api_key | ✅ 2465 ms | ⏭ | World Database on Protected Areas |
| [Map of Life API](https://mol.org/api) (`map_of_life`) | Map of Life | REST | global | — | ✅ 755 ms | ❌ JSONDecodeError: Expecting value: line 1 |  |
| [Movebank (suivi GPS d'animaux)](https://www.movebank.org/movebank/service) (`movebank`) | Max Planck Institute of Animal Behavior | REST | global | — | ❌ timeout after 20s | ❔ | Animal tracking via the movebank.py plugin. productType = a numeric study id → GPS fixes (points per individual); a non-numeric value → the public study list (one point per study). Most studies need a free Movebank login (auth.credentials) + licence acceptance; the public API is rate-limited. |

## 🪨 Geology

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [BRGM géologie 1:1M (INSPIRE)](http://mapsref.brgm.fr/wxs/1GG/BRGM_1M_INSPIRE_geolUnits_geolFaults) (`brgm_geologie`) | BRGM | WFS | france | — | ✅ 134 ms | ✅ 1 feat | Geological units and faults |
| [BRGM InfoTerre géoservices](http://geoservices.brgm.fr/geologie) (`brgm_infoterre`) | BRGM | WMS | france | — | ✅ 2169 ms | ✅ raster 146 × 105 |  |
| [OneGeology (portail mondial)](https://portal.onegeology.org/) (`onegeology`) | OneGeology | REGISTRY | global | — | ✅ 251 ms | ⏭ |  |
| [Macrostrat API](https://macrostrat.org/api) (`macrostrat`) | UW-Madison | REST | global | — | ✅ 421 ms | ❌ HTTPError: 500 Server Error: Internal Se |  |
| [USGS State Geologic Map (SGMC)](https://mrdata.usgs.gov/services/sgmc) (`usgs_sgmc`) | USGS | WMS | usa | — | ✅ 725 ms | ❌ HTTPError: 404 Client Error: Not Found f |  |
| [British Geological Survey](https://map.bgs.ac.uk/arcgis/services/) (`bgs_uk`) | BGS | REGISTRY | uk | — | ✅ 227 ms | ⏭ |  |
| [Tectonic plate boundaries (Peter Bird PB2002)](https://raw.githubusercontent.com/fraxen/tectonicplates/master/GeoJSON) (`tectonic_plates`) | Hugo Ahlenius / Peter Bird | REST | global | — | ✅ 199 ms | ✅ 54 feat | Peter Bird (2003) tectonic plate model as static GeoJSON, whole world per file (no bbox filter — clip client-side). Plates are polygons, boundaries are lines. |

## 🚧 Infrastructure

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [OpenSky Network (trafic aérien ADS-B)](https://opensky-network.org/api) (`opensky`) | OpenSky Network | REST | global | — | ✅ 119 ms | ✅ 73 feat | Live aircraft positions. Responses are positional ARRAYS (state vectors), so the shipped opensky.py plugin maps them to points (states/all). Anonymous access is rate-limited; a free OpenSky account (auth.credentials username/password) raises the limits. |
| [EIA (énergie & réseau électrique USA)](https://api.eia.gov/v2) (`eia`) | US Energy Information Administration | REST | usa | api_key | 🔐 | 🔐 | US energy data: power plants, generation, grid operations, prices. Free API key required (api_key query param). Tabular; geolocate facilities by plant id. |
| [WRI Global Power Plant Database](https://datasets.wri.org/dataset/globalpowerplantdatabase) (`global_power_plant`) | World Resources Institute | REGISTRY | global | — | ✅ 920 ms | ⏭ | About 35 000 power plants worldwide with capacity, fuel and lon/lat. Downloadable CSV (point data) — load directly; not a live API. |
| [Mapillary (photos de rue crowdsourcées)](https://graph.mapillary.com) (`mapillary`) | Meta / Mapillary | REST | global | api_key | ❌ HTTP 500 | ❔ | Street-level imagery coverage and derived map features. Free token (MLY_TOKEN in .env, sent as access_token); responses are JSON {data:[...]} with GeoJSON geometries. |
| [OpenCellID (antennes cellulaires)](https://opencellid.org) (`opencellid`) | Unwired Labs | REST | global | api_key | ✅ 138 ms | 🔐 | World's largest open database of cell towers (GSM/LTE/5G). Free key (OPENCELLID_KEY in .env); bulk CSV downloads also available per MCC. |
| [OpenRailwayMap (rail infrastructure tiles)](https://a.tiles.openrailwaymap.org/standard/{z}/{x}/{y}.png) (`openrailwaymap`) | OpenRailwayMap / OpenStreetMap | XYZ | global | — | ✅ 177 ms | ⏭ | Global railway network raster tiles from OSM (standard, maxspeed, signals, gauge styles). Overlay tiles (transparent PNG, served at 512 px); a User-Agent header is required (bare requests get HTTP 403). Underlying vector rail data is queryable via Overpass. |

## 👥 Population & demographics

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [World Bank Open Data](https://api.worldbank.org/v2) (`worldbank`) | World Bank | REST | global | — | ✅ 128 ms | ∅ 0 feature(s) | Socio-economic and demographic indicators by country and year — this is the source behind IntMap's choropleth indicators: GDP per capita / PIB par habitant, GDP growth, human development index (HDI) / IDH, life expectancy / esperance de vie, infant & under-5 mortality / mortalite infantile, fertility rate, unemployment / chomage, internet penetration / acces internet, urban population, electricity & safe-water access, CO2 per capita / emissions de CO2, inflation (CPI), Gini inequality, poverty / pauvrete, literacy / alphabetisation, and more (indicator codes like SP.POP.TOTL, NY.GDP.PCAP.CD). Tabular by ISO country code (no geometry) — join to admin boundaries (e.g. eurostat_gisco). |
| [WorldPop (densité de population)](https://api.worldpop.org/v1) (`worldpop`) | WorldPop / University of Southampton | REST | global | — | ✅ 1331 ms | ❌ JSONDecodeError: Extra data: line 10 col | Gridded population via the worldpop.py plugin: submits the bbox to the async stats API and returns one feature (bbox polygon + total_population). productType = dataset (default wpgppop). Rasters also download from hub.worldpop.org. |
| [US Census Bureau API (ACS)](https://api.census.gov/data) (`census_acs`) | US Census Bureau | REST | usa | api_key | ✅ 635 ms | 🔐 | US demographics and socio-economics by census geography (state/county/tract/block group). Free API key for sustained use (key= param). Returns rows keyed by FIPS — join to TIGER boundaries. |
| [Kontur Population (grille H3)](https://www.kontur.io/portfolio/population-dataset/) (`kontur_population`) | Kontur | REGISTRY | global | — | ✅ 1584 ms | ⏭ | Global population on a 400 m H3 hex grid (GeoPackage/GeoParquet download, CC-BY). Not a live API — download per country or worldwide. |

## 📚 Meta-catalogues & registries

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [Géocatalogue national (BRGM)](https://www.geocatalogue.fr/geonetwork/srv/fre/csw) (`geocatalogue_fr`) | BRGM (Géocatalogue national) | CSW | france | — | ✅ 136 ms | ⏭ | French national geographic metadata catalogue, operated by BRGM (datasets + WMS/WFS services) |
| [STAC Index (registre des API STAC)](https://stacindex.org/api/catalogs) (`stac_index`) | STAC community | REGISTRY | global | — | ✅ 283 ms | ⏭ | JSON list of all public STAC catalogs — ideal starting point |
| [GeoSeer (moteur de recherche OGC)](https://www.geoseer.net/) (`geoseer`) | GeoSeer | REGISTRY | global | — | ✅ 146 ms | ⏭ | Search engine over millions of WMS/WFS/WCS/WMTS layers |
| [INSPIRE Geoportal (catalogue UE)](https://inspire-geoportal.ec.europa.eu/srv/eng/csw) (`inspire_geoportal`) | European Commission / INSPIRE | CSW | europe | — | ❌ 200 but no capabilities document | ⏭ | Pan-European INSPIRE metadata catalogue (~380k records: datasets + services across all EU) |
| [data.gov catalogue (CKAN)](https://catalog.data.gov/api/3/action/package_search) (`datagov_us`) | US GSA | REST | usa | — | ❌ HTTP 404 | ❔ |  |
| [data.gouv.fr (portail open data France)](https://www.data.gouv.fr/api/1) (`datagouv_fr`) | Etalab / DINUM | REGISTRY | france | — | ✅ 165 ms | ⏭ | French national open-data portal (~50k datasets: DFCI, risques, PLU, réseaux...). `discover` keyword-searches the dataset API; records carry the dataset page and resource URLs (many are WMS/WFS/Atom feeds from geo-ide, shapefiles, GeoJSON). |
| [data.europa.eu (portail open data européen)](https://data.europa.eu/api/hub/search/ckan) (`data_europa_eu`) | Publications Office of the EU | REGISTRY | europe | — | ✅ 204 ms | ⏭ | Official EU open-data portal (~1.9M datasets harvested from every member state, CKAN-compatible search API). `discover` keyword-searches package_search; records carry distributions (downloads, some WMS/WFS). |
| [OpenDataSoft (réseau data.opendatasoft.com)](https://data.opendatasoft.com/api/explore/v2.1) (`opendatasoft_explore`) | OpenDataSoft | REGISTRY | global | — | ✅ 168 ms | ⏭ | Federated catalogue of every public OpenDataSoft portal (~35k datasets — many French collectivités: régions, départements, métropoles). `discover` keyword-searches the Explore API; every record gets a ready GeoJSON export URL. |
| [ArcGIS Hub (open data ArcGIS Online)](https://hub.arcgis.com/api/v3) (`arcgis_hub`) | Esri | REGISTRY | global | — | ✅ 544 ms | ⏭ | Search API over all public ArcGIS Online / ArcGIS Hub open data (millions of layers, incl. many French services). Records point straight at the ArcGIS REST FeatureServer layer (query with f=geojson). |

## Other

| Source | Provider | Type | Coverage | Auth | Status | Fetch | Notes |
|---|---|---|---|---|---|---|---|
| [OpenAerialMap](https://api.openaerialmap.org) (`openaerialmap`) | OpenAerialMap | REST | global | — | ✅ 465 ms | ∅ 0 feature(s) |  |
