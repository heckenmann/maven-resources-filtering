http://heckenmann.de/generieren-von-konfigurationsdateien-in-maven-projekten/

# maven-resources-filtering
Beispiel zum dynamischen Generieren von Konfigurationsdateien in Maven-Projekten. In diesem Beispiel werden Werte für verschiedene Umgebungen in eine Konfigurationsdatei ("MeineKonfiguration.xml") geschrieben.

Kann mit folgenden Befehlen gebaut werden:

## Testprofil
```
mvn clean install
```
oder
```
mvn clean install -P test
```

## Produktivprofil
```
mvn clean install -P prod
```
