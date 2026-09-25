# WueMedGuide

**WueMedGuide** ist eine digitale Lernzielplattform für Studierende der Humanmedizin.

Die Anwendung bündelt medizinische Lernziele in einer strukturierten, semester- und fachbezogenen Übersicht und unterstützt Studierende dabei, Lerninhalte gezielt zu organisieren, offene Themen zu identifizieren und den eigenen Lernfortschritt nachzuverfolgen.

## Öffentliche Frontend-Version
Aktuell erreichbar unter:

```text
https://wuemedguide.github.io/portal/
```

**Projektstatus:** aktive Entwicklung / Pilotversion

---

# Funktionen

Die Lernziele werden hierarchisch dargestellt:

```text
Vorklinik / Klinik
→ Semester
→ Disziplin
→ Vorlesung
→ Subgruppe
→ Lernziel
```

Aktuell stehen unter anderem folgende Funktionen zur Verfügung:

* strukturierte Darstellung aller Lernziele
* Gliederung nach Vorklinik und Klinik
* Semesterzuordnung
* Disziplinen
* Vorlesungen
* thematische Subgruppen
* Suche nach Disziplin, Vorlesung, Subgruppe oder Lernziel
* Filter „Nur offene“
* Filter „★ wichtig“
* Filter „🔁 wiederholen“
* Markierung von Lernzielen als erledigt
* Markierung wichtiger Lernziele
* Markierung zur Wiederholung
* Gesamtfortschrittsanzeige
* Fortschrittsanzeige innerhalb der einzelnen Ebenen
* Auswahl des nächsten offenen Lernziels
* zufällige Auswahl eines Lernziels
* Spotlight-/Fokusansicht für ausgewählte Lernziele
* persönliche Outline-Notizen pro Vorlesung
* responsive Darstellung für Desktop- und Mobilgeräte
* Zurücksetzen des lokal gespeicherten Lernzielstatus

---

# Technischer Aufbau

Die aktuelle öffentliche Version ist eine clientseitige Webanwendung und wird über **GitHub Pages** bereitgestellt.

Verwendete Technologien:

```text
HTML
CSS
JavaScript
GitHub Pages
Google Sheets
OpenSheet
Local Storage
Session Storage
```

Die öffentliche Frontend-Version verwendet aktuell **kein produktives serverseitiges Backend**.

Die Anwendungslogik läuft im Browser des jeweiligen Nutzers.

---

# Projektstruktur

```text
portal/
├── index.html
├── style.css
├── script.js
├── semester_map.json
└── README.md
```

## `index.html`

Enthält unter anderem:

* Grundstruktur der Website
* WueMedGuide-Branding
* Zugangsscreen
* Suchfeld
* Filter
* Buttons für Lernmodus und Zufallsauswahl
* Fortschrittsanzeige
* Container für die dynamische Lernzielstruktur
* clientseitige Passwort-Zugangshürde

## `style.css`

Enthält:

* Layout
* responsive Darstellung
* Karten- und Semesterdesign
* Gestaltung der Lernziele
* Fortschrittsanzeigen
* Filter- und Buttondesign
* Vorlesungsnotizen
* mobile Anpassungen

## `script.js`

Enthält die zentrale Anwendungslogik:

* Laden der Lernzieldaten
* Aufbau der Lernzielhierarchie
* Semesterzuordnung
* Suche
* Filter
* Fortschrittsberechnung
* Markierungen
* Spotlight-Funktion
* Zufallsauswahl
* Vorlesungsnotizen
* lokale Speicherung
* Wiederherstellung geöffneter Bereiche
* stabile interne IDs
* Farbzuordnung der Disziplinen

## `semester_map.json`

Enthält die Zuordnung von Disziplinen zu:

```text
Vorklinik / Klinik
Semester 1–10
```

---

# Datenquelle

Die eigentlichen Lernzieldaten sind nicht statisch direkt im GitHub-Repository hinterlegt.

Die Lernziele werden in **Google Sheets** gepflegt und über **OpenSheet** als JSON-Datenquelle für die Website verfügbar gemacht.

Aktueller OpenSheet-Endpunkt:

```text
https://opensheet.elk.sh/1PhAEGnH0KKUjRiA9vzVkAKICdze8L8d8BPGaTzyYQes/WebExport2.0
```

Die Anwendung lädt diese Daten beim Start der Website automatisch.

Das Google Sheet beziehungsweise der entsprechende WebExport enthält die folgenden relevanten Spalten:

```text
Disziplin
Vorlesung
Subgruppe
Lernziel
```

Die Daten werden anschließend vollständig clientseitig im Browser verarbeitet.

## Vorteil dieser Struktur

Änderungen an den Lernzieldaten können über das zugrunde liegende Google Sheet vorgenommen werden, ohne für jede inhaltliche Änderung das Frontend neu programmieren zu müssen.

Solange:

* der OpenSheet-Endpunkt erreichbar bleibt
* der Tabellenname beziehungsweise Export bestehen bleibt
* die erwarteten Spaltennamen unverändert bleiben

kann das Frontend die aktualisierten Daten automatisch laden.

## Abhängigkeit

Die öffentliche Website ist damit für die Lernzieldaten zusätzlich von der Verfügbarkeit von:

```text
Google Sheets
OpenSheet
```

abhängig.

---

# Semesterzuordnung

Die Semesterstruktur wird separat über:

```text
semester_map.json
```

geladen.

Die Datei definiert unter anderem:

```text
Vorklinik
Semester 1–4

Klinik
Semester 5–10
```

und ordnet einzelne Disziplinen den entsprechenden Semestern zu.

Falls `semester_map.json` nicht geladen werden kann oder kein gültiges Mapping enthält, besitzt WueMedGuide einen Fallback:

```text
Lernziele werden weiterhin direkt nach Disziplinen dargestellt.
```

Disziplinen, die zwar in den Lernzieldaten enthalten sind, aber nicht im Semester-Mapping vorkommen, können separat unter:

```text
Ohne Semesterzuordnung
```

angezeigt werden.

---

# Suche

Das Suchfeld durchsucht gleichzeitig:

```text
Disziplin
Vorlesung
Subgruppe
Lernziel
```

Die Suche kann mit den anderen Filtern kombiniert werden.

---

# Filter

Aktuell stehen drei zentrale Filter zur Verfügung:

## Nur offene

Blendet bereits erledigte Lernziele aus.

Dieser Modus kann als Lernmodus verwendet werden.

## Nur ★ wichtig

Zeigt ausschließlich Lernziele, die vom Nutzer als wichtig markiert wurden.

## Nur 🔁 wiederholen

Zeigt ausschließlich Lernziele, die zur Wiederholung markiert wurden.

Alle Filter können gemeinsam mit der Suchfunktion verwendet werden.

---

# Lernfortschritt

Für jedes Lernziel können aktuell drei Zustände gespeichert werden:

```text
done
star
repeat
```

entsprechend:

```text
erledigt
wichtig
wiederholen
```

Die Website berechnet daraus unter anderem:

* Anzahl erledigter Lernziele
* Gesamtzahl der Lernziele
* Anzahl wichtiger Lernziele
* Anzahl zur Wiederholung markierter Lernziele
* prozentualen Gesamtfortschritt
* Fortschritt pro Studienabschnitt
* Fortschritt pro Semester
* Fortschritt pro Disziplin
* Fortschritt pro Vorlesung

Die entsprechenden Anzeigen werden beim Bearbeiten eines Lernziels aktualisiert.

---

# Nächstes offenes Lernziel

Über:

```text
Nächstes offenes
```

kann innerhalb der aktuell gesetzten Suche und Filter das nächste noch offene Lernziel ausgewählt werden.

Das ausgewählte Lernziel erscheint anschließend in einer Spotlight-Ansicht.

---

# Zufälliges Lernziel

Über:

```text
Zufällig
```

wird zufällig ein Lernziel aus den aktuell durch Suche und Filter verfügbaren Lernzielen ausgewählt.

Dadurch kann beispielsweise ein zufälliges Lernziel zur Wiederholung ausgewählt werden, wenn gleichzeitig der Filter:

```text
Nur 🔁 wiederholen
```

aktiviert ist.

---

# Spotlight-/Fokusansicht

Ausgewählte Lernziele können in einer eigenen Fokusansicht dargestellt werden.

Dort werden angezeigt:

* Disziplin
* Vorlesung
* Subgruppe
* Lernziel
* Erledigt-Status
* Wichtig-Markierung
* Wiederholungs-Markierung

Direkt aus der Fokusansicht kann ein Lernziel:

* in der Liste angezeigt
* als erledigt/offen markiert
* als wichtig markiert
* zur Wiederholung markiert

werden.

Beim Sprung zu einem Lernziel öffnet WueMedGuide automatisch die benötigten übergeordneten Bereiche und scrollt zum entsprechenden Lernziel.

---

# Persönliche Vorlesungsnotizen

Zu jeder Vorlesung kann eine persönliche Outline-Notiz angelegt werden.

Beispiel:

```text
- Kerngedanke
  - Detail
  - Beispiel

- Prüfungsrelevant
  - ...

- Nacharbeiten
  - ...
```

Die Notizen werden lokal im Browser gespeichert.

Während der Eingabe werden Änderungen nach kurzer Verzögerung automatisch gespeichert.

Die `Tab`-Taste kann innerhalb des Notizfeldes für Einrückungen verwendet werden.

Dadurch können einfache hierarchische Outline-Notizen erstellt werden.

---

# Lokale Speicherung

Die öffentliche Frontend-Version speichert den individuellen Lernfortschritt ausschließlich im Browser des jeweiligen Nutzers.

## Lernzielstatus

Aktueller Local-Storage-Key:

```text
lernziele_state_v2
```

Gespeichert werden:

```text
done
star
repeat
```

## Vorlesungsnotizen

Separater Local-Storage-Key:

```text
lernziele_lecture_notes_v1
```

## Ältere Speicherstände

Die Anwendung enthält eine Migrationslogik für ein früheres Speicherformat:

```text
lernziele_done_v1
```

Falls ein älterer Speicherstand vorhanden ist, können bereits gespeicherte Erledigt-Markierungen in das neue Format übernommen werden.

---

# Einschränkungen der lokalen Speicherung

Da die öffentliche Version aktuell keine serverseitige Nutzerverwaltung besitzt:

* gibt es kein persönliches Nutzerkonto
* erfolgt keine Synchronisation zwischen Geräten
* ist der Fortschritt an den jeweiligen Browser gebunden
* können Daten beim Löschen der Browserdaten verloren gehen
* können Daten nicht zentral wiederhergestellt werden

Beispiel:

```text
Laptop → eigener Speicherstand

Smartphone → separater Speicherstand
```

---

# Zurücksetzen

Über:

```text
Zurücksetzen
```

können die gespeicherten Lernziel-Markierungen im aktuellen Browser zurückgesetzt werden.

Davon betroffen sind insbesondere:

```text
erledigt
wichtig
wiederholen
```

Die Vorlesungsnotizen werden separat gespeichert und sind nicht Teil dieses Status-Resets.

---

# Interne Identifikation der Lernziele

Damit gespeicherte Zustände zuverlässig einem Lernziel zugeordnet werden können, erzeugt WueMedGuide stabile interne Schlüssel.

Ein Lernziel wird aus der Kombination:

```text
Disziplin
Vorlesung
Subgruppe
Lernziel
```

identifiziert.

Aus dieser Kombination wird ein stabiler Hash erzeugt.

Beispiel eines internen Schlüssels:

```text
k234473e4
```

Vorlesungsnotizen erhalten auf vergleichbare Weise einen eigenen Schlüssel aus:

```text
Disziplin
Vorlesung
```

Dadurch müssen lange Lernzieltexte nicht direkt als Local-Storage-Schlüssel verwendet werden.

---

# Erhalt des geöffneten Arbeitsbereichs

Beim Ändern eines Lernzielstatus muss ein Teil der Oberfläche neu gerendert werden.

Damit dabei nicht alle geöffneten Ebenen zuklappen, merkt sich WueMedGuide vor dem Rendern, welche Bereiche aktuell geöffnet sind.

Das betrifft beispielsweise:

```text
Vorklinik / Klinik
Semester
Disziplin
Vorlesung
Outline-Notiz
```

Nach dem erneuten Rendern werden diese Bereiche automatisch wieder geöffnet.

Dadurch bleibt der aktuelle Lernkontext erhalten.

---

# Farbzuordnung der Disziplinen

Jede Disziplin erhält eine eigene Pastell-Farbkombination.

Ein Teil der Farben kann explizit bestimmten Disziplinen zugeordnet werden.

Weitere Disziplinen erhalten automatisch eine Farbe aus einer definierten Farbpalette.

Sollten mehr Disziplinen vorhanden sein als Farben in der hinterlegten Palette, können zusätzliche Pastellfarben automatisch generiert werden.

Die Farbcodierung dient ausschließlich der visuellen Orientierung.

---

# Zugang zur aktuellen Version

Die aktuelle öffentliche Version besitzt eine einfache clientseitige Passwortabfrage.

Die erfolgreiche Eingabe wird für die aktuelle Browser-Sitzung über:

```text
sessionStorage
```

gespeichert.

Dadurch muss das Passwort innerhalb derselben Browser-Sitzung nicht bei jedem erneuten Seitenaufruf eingegeben werden.

## Wichtiger Sicherheitshinweis

Diese Passwortabfrage stellt **keine echte serverseitige Authentifizierung** dar.

Da die Zugangslogik vollständig im Browser ausgeführt wird, dient sie ausschließlich als:

```text
einfache Zugangshürde für die aktuelle Pilotversion
```

Sie darf nicht zum Schutz vertraulicher oder sensibler Inhalte verwendet werden.

Für eine spätere produktive Version ist eine echte Nutzeridentifikation über:

```text
WueLogin
Shibboleth
SAML2
```

vorgesehen.

---

# Datenschutz und Nutzungshinweise

Die öffentliche Frontend-Version speichert persönlichen Lernfortschritt und Notizen lokal im Browser.

In den Notizfeldern dürfen keine:

* Patientendaten
* Gesundheitsdaten
* sensiblen personenbezogenen Daten
* vertraulichen Informationen

gespeichert werden.

Vor Einführung einer zentralen Speicherung ist eine entsprechende Datenschutz- und Betriebskonzeption vorgesehen.

---

# Lokale Entwicklung

Das Frontend kann lokal getestet werden.

Bei Verwendung des parallel entwickelten lokalen Backends ist die Anwendung beispielsweise erreichbar unter:

```text
http://localhost:3000
```

Die öffentliche GitHub-Pages-Version benötigt für ihre grundlegende Frontend-Funktion kein Node.js-Backend.

---

# Deployment über GitHub Pages

Die aktuelle öffentliche Frontend-Version wird über GitHub Pages bereitgestellt.

Typischer Workflow:

```bash
git add .
git commit -m "Update WueMedGuide frontend"
git push
```

Nach dem Push stellt GitHub Pages die neue Version bereit.

---

# Browser-Cache bei Updates

CSS- und JavaScript-Dateien können vom Browser zwischengespeichert werden.

Deshalb werden aktuell Versionsparameter verwendet, beispielsweise:

```html
<link rel="stylesheet" href="style.css?v=8" />
```

und:

```javascript
script.src = "script.js?v=8";
```

Nach größeren Änderungen kann die Versionsnummer erhöht werden:

```text
v=8
→
v=9
```

Dadurch behandelt der Browser die Datei als neue Ressource und lädt sie erneut.

Alternativ kann während der Entwicklung ein Hard Reload verwendet werden:

```text
Strg + F5
```

oder:

```text
Strg + Shift + R
```

---

# Aktueller Entwicklungsstand

Die öffentliche Version ist als funktionsfähiger Frontend-Prototyp verfügbar.

## Bereits umgesetzt

* öffentliche Bereitstellung über GitHub Pages
* Google-Sheets-basierte Lernzieldaten
* OpenSheet-Schnittstelle
* Vorklinik-/Klinik-Struktur
* Semesterstruktur
* Disziplinen
* Vorlesungen
* Subgruppen
* Lernziele
* Suche
* Filter
* Lernmodus
* Fortschrittsanzeige
* Markierung „erledigt“
* Markierung „wichtig“
* Markierung „wiederholen“
* nächstes offenes Lernziel
* Zufallsauswahl
* Spotlight-Funktion
* Vorlesungsnotizen
* lokale Speicherung
* responsive Darstellung
* einfache Zugangshürde
* WueCampus-Verlinkung
* Evaluation / Nutzerfeedback vorbereitet

---

# Parallel entwickeltes Backend

Zusätzlich zur öffentlichen GitHub-Pages-Version existiert ein Backend-Prototyp auf Basis von:

```text
Node.js
Express
REST-API
```

Dieser wurde lokal bereits erfolgreich getestet.

Der Backend-Prototyp kann unter anderem:

```text
GET  /api/health
GET  /api/user-data
POST /api/user-data
```

und damit:

* den Backend-Status prüfen
* Lernfortschritt laden
* Lernfortschritt speichern
* Vorlesungsnotizen laden
* Vorlesungsnotizen speichern
* das Frontend ausliefern

Die aktuelle Backend-Testversion verwendet noch einen gemeinsamen:

```text
demo-user
```

und ist daher noch nicht für einen produktiven Mehrnutzerbetrieb vorgesehen.

---

# Geplante produktive Architektur

Langfristig ist folgende Architektur vorgesehen:

```text
Browser
    ↓
JMU-Domain / HTTPS
    ↓
Webserver
    ↓
WueLogin / Shibboleth / SAML2
    ↓
WueMedGuide Backend
    ↓
Datenbank
```

Geplante Weiterentwicklungen:

* Hosting auf JMU-Infrastruktur
* virtuelle Maschine / Uni-Server
* HTTPS
* feste Domain / Subdomain
* WueLogin
* Shibboleth / SAML2
* eindeutige Nutzeridentifikation
* persönliche Nutzerkonten
* geräteübergreifender Lernfortschritt
* geräteübergreifende Notizen
* serverseitige Datenbank
* Backup-Konzept
* Löschkonzept
* Datenschutzkonzept
* VVT / datenschutzrechtliche Dokumentation
* produktiver Mehrnutzerbetrieb

---

# Abgrenzung Frontend und zukünftige Server-Version

## Aktuelle öffentliche Version

```text
GitHub Pages
+
Google Sheets / OpenSheet
+
Local Storage
```

Eigenschaften:

```text
kein echter Nutzerlogin
keine zentrale Speicherung
keine geräteübergreifende Synchronisation
```

## Geplante produktive Version

```text
JMU-Server
+
Backend
+
Shibboleth / WueLogin
+
Datenbank
```

Eigenschaften:

```text
persönliche Nutzeridentifikation
zentrale Speicherung
geräteübergreifender Zugriff
professioneller Serverbetrieb
```

---

# Projektstatus

**Status:** aktive Entwicklung / Pilotversion

WueMedGuide wird schrittweise weiterentwickelt und anhand von Rückmeldungen der Studierenden evaluiert.

---

# WueMedGuide

**Digitale Lernzielplattform für das Medizinstudium**

Öffentliche Frontend-Version:

```text
https://wuemedguide.github.io/portal/
```

Lernziel-Datenquelle über OpenSheet:

```text
```

