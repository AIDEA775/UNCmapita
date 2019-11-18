# UNCmapita

Una app Android para recorrer el campus universitario de la UNC.


## Este repo

Usamos OpenStreetMap (OSM), por sus principios.

Usamos MapBox para mostrar mapas de OSM.

MapBox tiene una libreria para Android que usa OpenGL que logra una experiencia muy fluida.

La unica forma que encontré hasta ahora de sobreponer informacion en mapbox es con un geojson.

[GeoJson](https://geojson.org/) no es compatible con todas las features que tiene OSM, pero se
puede convertir de un formato a otro.


## Workflow propuesto

1. Mapear Ciudad Universitaria (CU) en OSM.

1.  Descargar los mapas en formato .osm:
https://www.openstreetmap.org/export#map=19/-31.43791/-64.19329&layers=ND

1. Transformar .osm a .geojson usando osmium:
https://osmcode.org/osmium-tool/

   ```bash
   $ osmium export -o data.geojson map.osm
   ```

1. Levantar el .geojson usando mapbox.

1. Si hay que buscar algo, un aula por ejemplo, los datos están en el geojson.


## OpenStreetMap

OSM es una colección de etiquetas (Features).
Hubo varias propuestas sobre como taggear el interior de los edificios, pero
[esta](https://wiki.openstreetmap.org/wiki/Simple_Indoor_Tagging)
parece ser la que finalmente va a convertirse en el estandar.

Para ver mapas internos en la web:
https://openlevelup.net/?l=0#17/-31.43676/-64.19073

Para editar mapas internos en la web (iD):
https://projets.pavie.info/id-indoor/#background=MAPNIK&level=0&map=21.52/-64.19329/-31.43814

Para editar mapas internos no en la web (JOSM):
https://josm.openstreetmap.de/

Hay bastantes mapas indoor:
https://taginfo.openstreetmap.org/keys/indoor#map


## Más links

OSMAND es una app y hay un issue donde discuten soportar mapas indoor:
https://github.com/osmandapp/Osmand/issues/3559

Un hilo de Reddit donde hablan un poco de la comunidad y la Fundación OSM:
https://www.reddit.com/r/openstreetmap/comments/825x8s/indoor_editors/

Routing en indoor, pero no es prioritario:
https://help.openstreetmap.org/questions/9984/is-there-an-osm-indoor-routing-engineapi-for-android

En algun lado leí que se puede usar overpass para buscar datos:
http://overpass-turbo.eu/

Bocha de utilidades para geojson:
https://github.com/tmcw/awesome-geojson#readme

## Algunas ideas recientes:

* Tal vez colaborar con OsmAnd y soportar directamente los tags de OSM.

* Tal vez colaborar en MapBox para que soporte los tags.

* Usar otra lib que consuma directamente los datos de OSM, donde podamos soportar los tags?
 https://wiki.openstreetmap.org/wiki/Frameworks
