# UNCmapita

[![Join the chat at https://gitter.im/UNCmapita/community](https://badges.gitter.im/UNCmapita/community.svg)](https://gitter.im/UNCmapita/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Mapeando el campus universitario de la Universidad Nacional de Córdoba, Argentina (UNC).

## Updates! Leer abajo

## Este repo

Usamos OpenStreetMap (OSM), por sus principios y libertades.

Usamos MapBox para mostrar mapas de OSM.

MapBox tiene librerias para Android y Web que usan OpenGL y logran una experiencia muy fluida.

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

1. Si hay que buscar algo, un aula por ejemplo, los datos ya están en el geojson.

1. Si queremos la oficina de un docente, usar un servidor aparte.


## OpenStreetMap

OSM es una colección de etiquetas XML (Features) que describen cosas que hay en el mundo real.

Hubo varias propuestas sobre como taggear el interior de los edificios, pero
[esta](https://wiki.openstreetmap.org/wiki/Simple_Indoor_Tagging)
parece ser la que finalmente va a convertirse en el estandar.

Para ver mapas internos en la web:
https://openlevelup.net/?l=0#17/-31.43676/-64.19073

Para editar mapas internos en la web (iD):
https://projets.pavie.info/id-indoor/#background=MAPNIK&level=0&map=21.52/-64.19329/-31.43814

Para editar mapas internos NO en la web (JOSM):
https://josm.openstreetmap.de/

Hay bastantes mapas indoor:
https://taginfo.openstreetmap.org/keys/indoor#map


## Más links

OSMAND es una app y hay un issue donde discuten soportar mapas indoor:
https://github.com/osmandapp/Osmand/issues/3559

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


# 2019-12-22: Update 1:
 
## Mapeando FAMAF

1. Bajar lo planos internos de la facu desde acá:
https://www.famaf.unc.edu.ar/noticias/higiene-y-seguridad/

1. Bajar los _tiles_ de OSM correspondientes de la facu, como dice 
[acá](https://help.openstreetmap.org/questions/40163/where-can-i-find-out-tiles-number)
desde [acá (link al mapa)](https://mc.bbbike.org/mc/?lon=-64.193252&lat=-31.43807&zoom=19&num=1&mt0=mapnik).

   Ejemplo:
   
   ![osm tile](https://a.tile.openstreetmap.org/19/168655/310416.png)

1. Como FAMAF no estaba bien alineado, se puede bajar las fotos satelitales.

   2. Ir a un visor o editor de OSM: https://www.openstreetmap.org/edit?editor=id#map=19/-31.43810/-64.19320
   
   2. Elegir un mapa satelital con el boton de capas (Ersi tiene muy buena nitidez).
   
   2. Abrir la consola de desarrollador (_Inspect_), pestaña _Network_, y ver los pedidos que se hacen.

   2. Van a encontrar _tiles_ como este:

      ![foto satelital](https://wayback.maptiles.arcgis.com/arcgis/rest/services/world_imagery/mapserver/tile/3201/19/310416/168655)

1. Meter todo en [GIMP](https://www.gimp.org/), y con mucha paciencia rearmar el mapa con los tiles y alinearlos.
(no voy a escribir como hice porque seguro hay una mejor forma, pero subí un ejemplo [acá](https://blog.aidea775.com/map/out.xcf)).

1. Con los nuevos tiles listos, ir a 
https://projets.pavie.info/id-indoor/#background=Bing&level=0&map=19.37/-64.19322/-31.43811

1. Cambiar el mapa de fondo por uno [personalizado](https://projets.pavie.info/id-indoor/#background=custom:https://blog.aidea775.com/map/out/{zoom}_{y}_{x}.png&level=0&map=19.37/-64.19322/-31.43811):

   * Yo usé un server local, así que puse: http://localhost:1313/map/out/{zoom}_{y}_{x}.png

   * Lo subí a mi blog, pueden probarlo con https://blog.aidea775.com/map/out/{zoom}_{y}_{x}.png

1. Si no está alineado, pueden corregirlo con las opciones que salen mas abajo.

1. Estoy dudando de la fotos satelitales, se me hacen que también están desalineadas,
y los edificios no están tomados exactamente desde arriba, por lo que marcar el techo tampoco vale.

1. A mapear!

1. No! Pará! Hay que leer un poco sobre los tags, como mapear los pasillos y las puertas:
https://wiki.openstreetmap.org/wiki/Simple_Indoor_Tagging

## Otras ideas

* Una vez mapeada a CU, creemos que NO es común que los edificios cambien,
por lo que una app con un archivo estatico (el geojson) no suena a tan mala idea.

   Pero, no está bueno poner en OSM datos como los nombres de los docentes y sus oficinas (y que sea indexable).
   Por lo que a esos datos podríamos meterlo en un servidor aparte, donde poder hacer consultas,
   y ¿contener más info que no cuadra en OSM? ¿como cuales?.

* Lo groso sería tener el mapa en la web de la UNC: https://www.unc.edu.ar/
lo que nos lleva a pensar en usar efectivamente Mapbox (para darle el estilo de la UNC) y geojson.

* ¿Porque usar OSM y no directamente crear los mapas con un editor geojson?

   Porque ya que vamos a hacer un esfuerzo, OSM va a permitir que otros proyectos aprovechen de los datos que generemos
   con garantía de libertad y en un formato estándar. Y además, nos aprovechamos de sus herramientas de mapeo.
   

# Participar

Si tenes ganas de colaborar suscribite a esta lista de emails:
https://groups.google.com/d/forum/uncmapita.

Y también unite al Gitter:
https://gitter.im/UNCmapita/community
