# Filtern von Konfigurationsdateien in Maven-Projekten

In den meisten Projekten existieren Konfigurationsdateien, die diverse Parameter für die Applikation enthalten, wie z.B. die IP der Datenbank, auf die zugegriffen werden soll. Doch was geschieht, wenn man in der Entwicklungsumgebung seine Änderungen testen möchte? Es wäre nicht klug, dazu die Produktivdatenbank zu verwenden. Aus diesem Grund haben die Entwickler oft eine lokale Datenbank installiert oder verwenden zum Testen eine zentrale Testdatenbank. Sind die Konfigurationsdateien statisch, bleibt dem Entwickler keine andere Möglichkeit, als die Parameter von Hand zu ändern, was einerseits Mehraufwand bedeutet und andererseits fehleranfällig ist. Falls es sich nur um die Adresse der Datenbank handelt, möge das noch kein Problem darstellen. Enthält das Projekt viele Dateien mit einer großen Anzahl an Parametern, wird diese Arbeit schnell unüberschaubar.


Dieses Problem lässt sich einfach durch Filtering mit dem Maven-Resources-Plugin lösen. Alle möglichen Konfigurationsparameter werden zu Beginn in der pom.xml als Property angelegt. Dann wird für jedes Einsatzzenario ein Maven-Profil erstellt. Z.B. jeweils ein Profil für Entwicklungsumgebung, Testumgebung und Produktivumgebung. Falls unterschiedliche Produktivsysteme existieren, kann dieses Vorgehen auch dafür verwendet werden. In den Profilen werden die Werte so gesetzt, wie sie später in der Konfigurationsdatei auftauchen sollen. Hat man im Properties-Abschnitt des pom z.B. das Property "dev.db.ip" (für die Entwicklerdatenbank) (kann beliebig gewählt werden) gesetzt, kann im Profil für die Entwicklungsumgebung nun das Property "current.db.ip" auf "dev.db.ip" verweisen. Analog dazu z.B. auch "current.db.ip" im Produktivprofil auf "prod.db.ip" (für eine Produktivdatenbank).
Damit das Maven-Plugin weiß, an welcher Stelle es die Werte einsetzen soll, trägt man in die Konfigurationsdateien die Platzhalter ein. In diesem Fall ${current.db.ip} an der Stelle, an der die IP der Datenbank stehen muss.

Nun muss der Entwickler nur
```
mvn clean install -P dev
```
oder
```
mvn clean install -P prod
```
aufrufen und schon wird ihm ein Archiv mit der korrekten Konfiguration gebaut. Verwendet man Jenkins CI oder sonst ein CI-System, können mit diesem Vorgehen auch darüber leicht Versionen für verschiedene Umgebungen gebaut werden.

**Hinweis:** Das Plugin ändert natürlich nicht die Source-Code-Datei, sondern die Datei im target-Verzeichnis, die später im Archiv landet. Um das Ergebnis zu begutachten, öffnet man das Archiv, sucht die Datei, die gefiltert werden sollte, und prüft, ob die Werte korrekt eingesetzt worden.

Doch genug der verwirrenden Worte. Wie es funktioniert, sieht man im Beispiel-Code.

**Quellen:**
- http://maven.apache.org/plugins/maven-resources-plugin/examples/filter.html
- https://maven.apache.org/plugins/maven-war-plugin/examples/adding-filtering-webresources.html
