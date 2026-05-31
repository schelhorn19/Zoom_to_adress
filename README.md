# Zoom to Address — von der Adresse zum Geländemodell

> Ein R-Tool, das aus einer einfachen Adresse automatisch das passende digitale Geländemodell (DGM1) lädt und an den Standort heranzoomt — Geocoding, Kachel-Download und Visualisierung in einem Schritt.

![R](https://img.shields.io/badge/R-276DC3?style=flat&logo=r&logoColor=white)
![terra](https://img.shields.io/badge/terra-spatial-589632?style=flat)
![OpenData](https://img.shields.io/badge/Open%20Data-LGL%20BW-orange?style=flat)

---

## Worum geht es

Wer mit Geländedaten arbeitet, kennt den lästigen Zwischenschritt: Erst die Adresse in Koordinaten umwandeln, dann herausfinden, welche DGM-Kachel man braucht, sie manuell vom Geoportal herunterladen, entpacken, einlesen und projizieren — bevor man überhaupt etwas sieht.

Dieses Tool automatisiert die ganze Kette. Du gibst eine **Adresse** ein, und die Funktion liefert dir das **digitale Geländemodell (DGM1, 1 m Auflösung)** rund um diesen Punkt — inklusive Übersichts- und Zoom-Plots.

## Was das Skript macht

1. **Geocoding** — wandelt die Adresse über OpenStreetMap (`tidygeocoder`) in Koordinaten um.
2. **Projektion** — transformiert den Punkt nach UTM 32N (EPSG:25832) und Gauß-Krüger 3 (EPSG:31467).
3. **Kachel bestimmen** — leitet aus den Koordinaten automatisch den Namen der passenden DGM1-Kachel ab.
4. **Download & Entpacken** — lädt die Kachel aus den offenen Geodaten des [LGL Baden-Württemberg](https://opengeodata.lgl-bw.de) und prüft vorher, ob sie überhaupt existiert.
5. **Raster aufbauen** — liest die `.xyz`-Punktdaten ein, baut daraus ein `terra`-Raster und reprojiziert es.
6. **Visualisieren** — zeigt das gesamte DGM sowie zwei einstellbare Zoomstufen (Standard: 200 m und 50 m Radius) um die Adresse.

## Nutzung

```r
# Adresse als data.frame mit Spalte 'adresse'
adr <- data.frame(adresse = "Belfortstraße 15, 79098 Freiburg")

ergebnis <- meine_funktion(
  p_adresse   = adr,
  p_save_path = "C:/temp/dgm",
  p_buffer_1  = 200,   # äußerer Zoom (m)
  p_buffer_2  = 50     # innerer Zoom (m)
)

# Rückgabe: list(raster = <DGM>, punkt = <Adresspunkt>)
```

## Installation

```r
# Pakete installieren
source("install.R")
# oder manuell:
install.packages(c("tidygeocoder", "dplyr", "terra", "curl", "stringr"))
```

Repo-Dateien:

- `Script_zoom_to_adress.Rmd` — das R-Skript mit der Funktion `meine_funktion()`
- `Script_zoom_to_adress.ipynb` / `Script_zoom_to_adress2.ipynb` — Notebook-Varianten
- `Script_zoom_to_adress.nb.html` — gerendertes Notebook mit Beispiel-Plots
- `install.R`, `apt.txt`, `runtime.txt` — Setup (u.a. für Binder-Ausführung)

## Verwendete Werkzeuge

- **R** mit `tidygeocoder` (Geocoding), `terra` (Raster & Reprojektion), `dplyr`, `stringr`, `curl`
- **Offene Geodaten** des LGL Baden-Württemberg (DGM1)
- **Koordinatensysteme:** WGS84 (EPSG:4326), UTM 32N (EPSG:25832), Gauß-Krüger 3 (EPSG:31467)

## Hinweise

- Aktuell auf das DGM1-Kachelschema von **Baden-Württemberg** zugeschnitten; für andere Bundesländer müsste die URL-/Kachellogik angepasst werden.
- Geocoding-Genauigkeit hängt von OpenStreetMap ab; bei unklaren Adressen empfiehlt sich eine Plausibilitätsprüfung des Punkts.

## Über mich

Hydrogeologe & Data Scientist mit 9+ Jahren Erfahrung in Geomatik, Grundwassermodellierung und datengestützter Analyse mit R und Python. Mehr unter [github.com/schelhorn19](https://github.com/schelhorn19).
