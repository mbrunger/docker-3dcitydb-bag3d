# docker-3dcitydb-bag3d
Toelichting toevoegen BAG3D data aan 3DCityDB Docker installatie

### 3D City Database PostgreSQL/PostGIS 
- De [3DCityDB Postgresql/Postgis Docker container](https://hub.docker.com/r/3dcitydb/3dcitydb-pg) bevat een 3DCityDB installatie met een PostgreSQL/PostGIS database
-	De twee verplichte parameters bij het runnen van de container zijn een wachtwoord voor de database en het coördinatenstelsel waarin de gegevens worden opgeslagen. Ik heb hierbij gekozen voor SRID=28992. 
-	Via psql kun je verbinding maken met de database. 
	-	Voorbeeld van enkele standaardtabellen in de 3dCityDb database:
		- ![image](https://user-images.githubusercontent.com/47392146/162755890-49460672-a859-4c90-a116-91560d5ed85c.jpeg)
	- Voorbeeld van de structuur van een tabel: 
		- ![image](https://user-images.githubusercontent.com/47392146/162759916-ad22c69d-b458-4c13-be55-92f20b384a45.jpeg)

### Import/export applicatie 
-	De [3DCityDB Importer/Exporter Docker container](https://hub.docker.com/r/3dcitydb/impexp) bevat de impexp applicatie waarmee je 3D data kan importeren en exporteren. De Docker versie heeft geen grafische user interface maar alleen een command line interface.
-	Importeren/exporteren kan in CityGML of CityJSON formaat
-	Voor de visualisatie in de 3DCityDB Web Map Cesium viewer kan worden geëxporteerd naar KML/Collada/glTF formaat via de volgende stappen:
	1. Eerst heb ik een CityJSON bestand geïmporteerd: 
		 - Van https://3dbag.nl/en/download heb ik 1 tile met LOD2.2 gebouwen in CityJSON formaat gedownload
		 -	Met het import commando heb ik de tile geïmporteerd in de 3DCityDB database
		 -	Geïmporteerde data wordt opgeslagen in de tabel cityobject:
			 - ![image](https://user-images.githubusercontent.com/47392146/162762159-a52bd9fc-5e0f-40ca-88e6-6e964aa01cfa.jpeg)
	2. Vervolgens heb ik met het export-vis commando de data geëxporteerd naar Collada/glTF formaat. 
		 - Er worden dan een <file_name>_collada_MasterJson.json, een <file_name>.json en een <file_name>.kml aangemaakt. 
		 - Daarnaast wordt er een map Tiles aangemaakt met in dit geval 1 tile map met daarin per gebouw een map met o.a. een .gltf en een .dae bestand. 
		 - Het .dae bestand kan op de Mac ook direct als 3d gebouw worden weergegeven:
			 - ![image](https://user-images.githubusercontent.com/47392146/162763656-15e5de62-d12f-4da0-9a3c-ba9fd27067f2.jpeg)

### Web Map client
-	De [3DcityDB Web Map Client Docker container](https://hub.docker.com/r/tumgis/3dcitydb-web-map) bevat de 3DCityDB web map Cesium-client
-	Bij het runnen van de container kun je de map waar de geëxporteerde visualisatie bestanden staan opgeven. De bestanden zijn dan zichtbaar in de lijst met bestanden en kunnen toegevoegd worden aan de kaart:
	- ![image](https://user-images.githubusercontent.com/47392146/162765085-5f9a4e9a-8ecb-4594-9d75-4377efe40da7.jpeg)
-	Via de Toolbox kan een nieuwe laag geconfigureerd worden. De locatie van de drie bestanden uit de export (<file_name>_collada_MasterJson.json, <file_name>.json en <file_name>.kml) kunnen hier worden ingevuld:
	- ![image](https://user-images.githubusercontent.com/47392146/162765201-71fe1021-b937-4814-88b3-a182fbd3579c.jpeg)
-	De tile met de gebouwen wordt vervolgens in de viewer weergegeven:
	- ![image](https://user-images.githubusercontent.com/47392146/162765291-ba051441-3e1f-43fd-b44d-6d4bcb90db77.jpeg)
	- ![image](https://user-images.githubusercontent.com/47392146/162765333-64c64d06-888f-4953-a79a-fb998f4ef0dc.jpeg)
 
### WFS
-	De [3DCityDB Web Feature Service Docker container](https://hub.docker.com/r/3dcitydb/wfs) bevat de WFS functionaliteit
-	Als je de container runt geef je de database authenticatie gegevens mee. De WFS service zou dan de gegevens uit de database moeten bevatten.
-	Het GetCapabilities request (http://localhost:8080/citydb-wfs/wfs?service=WFS&request=GetCapabilities) geeft een overzicht van de lagen in de service:
	-	![image](https://user-images.githubusercontent.com/47392146/162765680-c6f9a39c-23f6-4171-a9d8-4d3179225bfb.jpeg)
-	De WFS-service kan ook verbinding maken in QGIS, maar de lagen hebben geen geografie. De 3D BAG gegevens lijken dus nog niet in de WFS te zitten, maar dit is een kwestie van goed configureren.
	- ![image](https://user-images.githubusercontent.com/47392146/162765740-85386fd4-4ec2-4bd3-aa9a-9ce1d31762c7.jpeg)
