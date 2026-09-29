# Wetter

ApexGPS kann aktuelle Bedingungen, eine stündliche Vorschau und eine 7-Tages-Aussicht für jeden Punkt anzeigen, an dem Sie interessiert sind.

## Was Sie sehen

Wetter ist seit 1.32.2 **standardmäßig aktiviert**. Sobald Sie einen GPS-Fix haben:

- Ein kleiner Chip erscheint über der Statusleiste und zeigt die Bedingungen an Ihrem aktuellen Standort.
- Beim Tippen auf einen Wegpunkt erscheint im Wegpunkt-Panel eine Zeile „Wetter hier“.
- Tippen auf eine der beiden Oberflächen öffnet ein Sheet mit der vollständigen Aufschlüsselung.

Wenn Wetter aktiviert ist, wird Ihr Breitengrad/Längengrad bei jeder Vorhersage-Abfrage an Open-Meteo (eine kostenlose öffentliche Wetter-API) gesendet. Wenn Sie keine Netzwerk-Aufrufe wünschen, öffnen Sie **Einstellungen → Wetter** und schalten Sie **Wetter anzeigen** aus — der Chip und die Zeile „Wetter hier“ verschwinden, und ApexGPS kontaktiert Open-Meteo nicht mehr.

## Was der Chip anzeigt

Der Chip wechselt zwischen einigen Zuständen:

| Zustand | Sieht aus wie | Bedeutung |
|---|---|---|
| Aktuell | ein Wettersymbol, dann `24° · 12 km/h NO` | Aktualisiert in den letzten 15 Minuten. |
| Älter | `… · vor 32 Min.` | Älter als 15 Minuten, aber wahrscheinlich noch genau. |
| Veraltet / offline | `⚠ … · vor 2 Std.` (ausgegraut) | Älter als eine Stunde oder keine Verbindung. Tippen Sie zum manuellen Aktualisieren. |

Der Chip verschwindet nicht, wenn Wetter aktiviert ist — er ändert nur sein Aussehen, um Ihnen zu zeigen, wie verlässlich die Daten sind.

## Das Vorhersage-Sheet

Tippen Sie auf den Chip (oder auf die „Wetter hier“-Zeile bei einem Wegpunkt), um das vollständige Sheet zu öffnen. Von oben nach unten:

- **Jetzt** — großes Wettersymbol + Temperatur, „gefühlt“ und je eine Zeile für Wind / Luftfeuchte + max. Niederschlag / Taupunkt + UV / Druck + Sonnenuntergang.
- **Vorhersage-Streifen mit 24h / 8h / 2h-Umschalter** — acht Symbole (Tag- oder Nachtvariante je nach lokalem Sonnenauf- und -untergang für diesen Schritt) mit der Temperatur und darunter der **Niederschlagswahrscheinlichkeit**. Der Prozentwert erscheint erst ab 20 %, damit eine trockene Vorhersage übersichtlich bleibt. Der kleine Umschalter über dem Streifen wechselt den Zeitraum:
  - **24h** — der ganze Tag auf einen Blick, eine Zelle alle 3 Stunden.
  - **8h** — die nächsten acht Stunden, eine Zelle pro Stunde.
  - **2h** — die nächsten zwei Stunden in 15-Minuten-Schritten, um einen aufziehenden Schauer oder ein Gewitter früh zu erkennen (bis zu ~45 Minuten früher als die stündliche Ansicht). **2h** erscheint dort, wo 15-Minuten-Daten verfügbar sind (große Teile Europas und Nordamerikas); sonst sehen Sie nur **24h** und **8h**.
- **Druckverlauf** (grün) — 24-Stunden-Liniendiagramm, hilfreich, um eine herannahende Front zu erkennen.
- **Luftfeuchte-Verlauf** (hellblau) — 24-Stunden-Liniendiagramm.
- **Nächste 7 Tage** — eine Reihe von Wochentag-Symbolen mit Höchst-/Tiefstwert des Tages sowie der Niederschlagswahrscheinlichkeit, sofern sie 20 % oder mehr erreicht.

Im Sheet-Header gibt es einen Aktualisieren-Button (↻), der den Cache umgeht und frische Daten abruft. Wenn Sie offline sind, bleiben die vorherigen Daten auf dem Bildschirm und der Veraltet-Indikator des Chips bleibt sichtbar.

## Höhenbewusste Vorhersagen auf Gipfeln

Wenn ein Wegpunkt eine gespeicherte Höhe hat (manuell eingegeben, vom GPS gesetzt oder aus einer GPX mit `<ele>` importiert), wird diese Höhe an Open-Meteo gesendet, damit das Modell weiß, dass es sich um einen Gipfel handelt und nicht um einen Talpunkt bei derselben Lat/Lon. Dasselbe gilt für den Chip: Er verwendet Ihre GPS-Höhe. Auf einem 1500 m hohen Gipfel korrigiert dies die Vorhersage-Temperatur typischerweise um ~9 °C im Vergleich zu einer reinen Koordinaten-Abfrage.

## Automatische Aktualisierung

Wenn Wetter aktiviert ist, aktualisiert der Chip alle 15 Minuten, solange die App geöffnet und online ist. Sind Sie im Flugmodus, behält der Chip die letzten bekannten Daten und einen „Veraltet“-Indikator, bis Sie wieder verbunden sind.

## Die Symbole lesen

Die Wettersymbole zeichnet ApexGPS selbst — sie sehen daher auf **jedem Telefon exakt gleich** aus. Die Farbe trägt
die Bedeutung:

| Farbe | Bedeutung |
|---|---|
| Blau | Niederschlag — Niesel, Regen, Schneeregen oder Schnee |
| Bernstein | Klarer Himmel (die Sonne; nachts zeigt klarer Himmel einen schlichten grauen Mond) |
| Grau | Wolken oder Nebel |
| Rot | Gewitter |

Die Symbole unterscheiden die Stärke: leichter Regen, Regen und starker Regen sind drei verschiedene Symbole, ebenso
Schnee, starker Schnee und Schneeregen. Da die Niederschlagswahrscheinlichkeit zusätzlich als Zahl erscheint, müssen
Sie sich nie allein auf ein kleines Symbol verlassen.

Bis Version 1.47.1 waren diese Symbole Emojis, die das Telefon selbst lieferte — zwei Geräte konnten deshalb bei
derselben Vorhersage unterschiedliche Bilder oder gar keines anzeigen. Ab 1.48.0 ist das behoben.

## Datenquellen

Die Vorhersagen kommen von **[Open-Meteo](https://open-meteo.com)**, einer kostenlosen öffentlichen API, die ECMWF, GFS, ICON und andere erstklassige globale Modelle kombiniert. Kostenlos für den persönlichen Gebrauch, kein Konto, kein API-Schlüssel.

## Einschränkungen

- **Konvektiver Regen in trockenen Regionen** ist für jedes Modell schwer vorherzusagen. Erwarten Sie gelegentliche Aussetzer bei Sturzflut-Gewittern in Regionen wie den emiratischen Hajar-Bergen. Die Wahrscheinlichkeit ist ehrlich darüber, aber lokalisierte Ereignisse können neben dem Raster liegen.
- **Unwetter-Warnungen** sind nicht Teil dieser Funktion. ApexGPS sendet keine Push-Nachrichten bei nahenden Stürmen.
- **Routen-Wetter** wird nicht unterstützt — Vorhersagen sind Punkt-Abfragen, nicht „Wie wird das Wetter entlang dieses Pfades in 2 Stunden“. Sie können einzelne Wegpunkte entlang einer Route antippen, um stichprobenartig zu prüfen.
