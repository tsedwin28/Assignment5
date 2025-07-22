# Assignment5
# Assignment 5: Markdown and Software Libraries  
---

## Case 1: Distance Between Two Polygon Centroids

**Task**: Calculate the distance between the centroids of two polygons in WKT format.

## Libraries Used
- [`Shapely`](https://shapely.readthedocs.io/en/stable/): A Python library for manipulation and analysis of planar geometric objects.

## Methods Used
- `wkt.loads()` to parse WKT strings
- `Polygon.centroid` to find centroids
- `centroid.distance()` to measure Euclidean distance

##  Code

```python
from shapely import wkt

wkt1 = "POLYGON ((4.2 0, 1.3 4, -3.4 2.47, -3.4 -2.47, 1.3 -4, 4.2 0))"
wkt2 = "POLYGON ((46.2 0, 43.3 4, 38.6 2.47, 38.6 -2.47, 43.3 -4, 46.2 0))"

polygon1 = wkt.loads(wkt1)
polygon2 = wkt.loads(wkt2)

centroid1 = polygon1.centroid
centroid2 = polygon2.centroid

distance = centroid1.distance(centroid2)

print("Distance:", round(distance, 2))
```

---

## Case 2: Coordinate Transformation (EPSG:4326 → EPSG:2326)

**Task**: Convert a WKT point from EPSG:4326 to EPSG:2326 (Hong Kong 1980 Grid).

## Libraries Used
- [`pyproj`](https://pyproj4.github.io/pyproj/stable/)
- [`Shapely`](https://shapely.readthedocs.io/en/stable/manual.html#shapely.ops.transform)

## Code

```python
from shapely import wkt
import pyproj
from shapely.ops import transform

wkt_point = "POINT (114.17208 22.29345)"
point = wkt.loads(wkt_point)

project = pyproj.Transformer.from_crs("EPSG:4326", "EPSG:2326", always_xy=True).transform
projected_point = transform(project, point)

print("Transformed (EPSG:2326):", projected_point)
```
**Challenges**: Navigating to the correct CRS code (EPSG:2326) is important for this use case.
---

## Case 3: Filter Nova Scotia Airports from GeoJSON

**Task**: Download a GeoJSON file and extract airport features located in Nova Scotia.

## Libraries Used
- [`GeoPandas`](https://geopandas.org/en/stable/)
- [`Pandas`](https://pandas.pydata.org/)

## Code

```python
import geopandas as gpd

url = "https://maps-cartes.services.geo.ca/server_serveur/rest/services/TC/canadian_airports_w_air_navigation_services_en/MapServer/0/query?outFields=*&where=1=1&f=geojson"
gdf = gpd.read_file(url)

filtered = gdf[gdf["PROVINCE"] == "NS"]
filtered.to_csv("nova_scotia_airports.csv", index=False)

print("Saved", len(filtered), "airports to CSV.")
```
**Challenges**: Struggled to read the full dataset, so it worked best when the dataset is read first before applying filters.
---

## Case 4: Reverse Geocoding

**Task**: Perform reverse geocoding on the original point from Use Case 2.

## Libraries Used
- [`geopy`](https://geopy.readthedocs.io/en/stable/#geopy.geocoders.Nominatim)

## Code

```python
from geopy.geocoders import Nominatim
from shapely import wkt

wkt_point = "POINT (114.17208 22.29345)"
point = wkt.loads(wkt_point)

geolocator = Nominatim(user_agent="unigis_assignment")
location = geolocator.reverse((point.y, point.x))

print("Address:", location.address if location else "No address found")
```
**Challenges**: Arrangement of latitude and longitude is problematic when done right. Should be (x, y) and not ( lat, long)
---
