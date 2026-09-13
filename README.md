# BJS-Tool

Ein lokales Tool zur Verwaltung der Bundesjugendspiele Leichtathletik an Schulen.

## Überblick

Die **Bundesjugendspiele Leichtathletik** sind ein bundesweiter Wettbewerb für Teilnehmerinnen und Teilnehmer aller allgemeinbildenden Schulen in Deutschland. Dieses Tool unterstützt Schulen bei der kompletten Organisation, Durchführung und Auswertung der Bundesjugendspiele – von der Teilnehmerverwaltung über die Wettkampfdurchführung bis hin zur automatischen Urkundenerstellung.

Das Programm ist als **lokale Anwendung** konzipiert und läuft vollständig auf einem Schulrechner ohne Internetverbindung. Es richtet sich an Sportlehrkräfte, Wettkampforganisatoren und Sekretariate, die die Bundesjugendspiele effizient und fehlerfrei durchführen möchten.

## Zielsetzung

### Problemstellung

Die Durchführung der Bundesjugendspiele an Schulen ist mit erheblichem organisatorischem Aufwand verbunden:

- **Stammdatenverwaltung**: Hunderte von Teilnehmern müssen erfasst und verwaltet werden
- **Wettkampfordnung**: Jeder Teilnehmer muss der richtigen Altersklasse (AK) und den korrekten Disziplinen zugeordnet werden
- **Riegenbildung**: Teilnehmer müssen in sinnvolle Gruppen (Riegen) eingeteilt werden
- **Ergebniserfassung**: Wettkampfergebnisse müssen schnell und fehlerfrei erfasst werden
- **Punkteberechnung**: Die Punktevergabe nach amtlichem Punktespiegel ist komplex und fehleranfällig
- **Urkundenerstellung**: Jeder Teilnehmer benötigt eine individuelle Urkunde (Teilnahme/Sieger/Ehrenurkunde)

### Lösung

Dieses Tool automatisiert alle diese Prozesse:

- **CSV-Import** der Teilnehmerstammdaten aus der Schulverwaltung
- **Automatische Zuordnung** zu Altersklassen und Disziplinen nach Geschlecht und Geburtsjahr
- **Flexible Riegenbildung** nach Klasse, Jahrgang, Geschlecht und Disziplin
- **Batch-Ergebniserfassung** mit Plausibilitätsprüfung
- **Automatische Punkteberechnung** nach aktuellem Punktespiegel
- **PDF-Urkunden** mit Filterfunktion für massenhaften Druck

## Features im Detail

### 1. Teilnehmerverwaltung

- **CSV-Import**: Stammdaten aus Excel/CSV der Schulverwaltung importieren
  - Unterstützte Formate: CSV, Excel (.xlsx)
  - Automatische Encoding-Erkennung (UTF-8, Latin-1, etc.)
  - Validierung der Pflichtfelder (Vorname, Nachname, Klasse, Geschlecht, Geburtsjahr)
- **Teilnehmerübersicht**: Alle Teilnehmer mit Such- und Filterfunktion
  - Anzeige nach Klasse sortiert
  - Doppelte Trennlinie bei Klassenwechsel
  - Zebra-Streifen für bessere Lesbarkeit
- **Datenqualität**: Plausibilitätsprüfung der importierten Daten

### 2. Wettkampfordnung

- **Automatische Klassifizierung**: Jeder Teilnehmer wird automatisch der richtigen Altersklasse (AK) zugeordnet
  - AK 6–17 basierend auf Geburtsjahr und Wettkampfjahr
  - Unterscheidung nach Geschlecht (m/w)
- **Disziplin-Zuordnung**: Sprint, Sprung, Wurf je nach Alter und Geschlecht
  - Konfiguration in `wettkampf.json` hinterlegt
  - Plausibilitätsgrenzen für Ergebnisvalidierung
- **Disziplinen-Matrix**: Tabellarische Eingabe (eine Zeile je
  Disziplinvariante, eine Spalte je Alter/Klassenstufe) statt einzelner
  Checkboxen - löst insbesondere Grenzjahrgänge (z. B. AK 13 zwischen
  Sprint 50m und 75m) durch eine bewusste Auswahl statt einer
  automatischen Regel auf. Sprung-Disziplinen (z. B. Weit- und
  Hochsprung) lassen sich dabei bewusst parallel auswählen, da sie laut
  Regelwerk gemeinsam gelten.
  - **Messgenauigkeit für Sprint (Zehntel-/Hundertstelsekunden)**: Eine
    Einstellung für alle Sprint-Disziplinen der Veranstaltung, direkt
    über der Matrix. Wirkt sich auf die Nachkommastellen in der
    Ergebniseingabe aus (siehe "Ergebniserfassung" unten) - bei
    elektronischer Zeitmessung werden unabhängig von dieser Einstellung
    immer Hundertstelsekunden verwendet.
- **Regelkonformität**: Entspricht der offiziellen Wettkampfordnung der Bundesjugendspiele

### 3. Riegenbildung

- **Flexible Kriterien**: Riegen können nach mehreren Kriterien gebildet werden
  - Klasse: Teilnehmer einer Klasse bleiben zusammen
  - Altersklasse: Nach Jahrgang gruppieren
  - Geschlecht: Getrennte Riegen für Jungen/Mädchen
  - Disziplin: Nach Wettkampfdisziplin (Sprint, Sprung, Wurf)
- **Sortierreihenfolge**: Kriterien per Drag&Drop anpassbar
  - Bestimmt die Riegennummern und -reihenfolge
  - Beispiel: Klasse → Geschlecht → Disziplin
- **Riegennummern**: Automatische Nummerierung als Suchschlüssel
- **Druckansicht**: Vereinfachte Riegenlisten zum Ausdrucken

### 4. Ergebniserfassung

- **Batch-Eingabe**: Alle Ergebnisse einer Riege auf einmal erfassen
  - Kassen-Eingabe: Nur Ziffern tippen, Komma steht fest
  - Beispiel: "9" "4" → "9,4" Sekunden
- **Plausibilitätsprüfung**: Unplausible Werte werden sofort orange markiert
  - Grenzwerte aus `wettkampf.json` (z.B. Weitsprung 2–6m)
  - Dynamisches Laden der Grenzwerte
  - Visuelle Rückmeldung beim Tippen
  - Diese Grenzwerte sind bewusst nur ein optischer Hinweis, damit sehr
    gute oder sehr schwache, aber reale Ergebnisse trotzdem gespeichert
    werden können
  - **In der Riegen-Übersicht** (Seite "Erfassung") wird zusätzlich zur
    rot/orange/grün-Grundfarbe jedes Riegenquadrat mit einem lila Rahmen
    und einem kleinen Warnsymbol markiert, wenn mindestens ein bereits
    gespeicherter Wert dieser Riege außerhalb der Plausibilitätsgrenzen
    liegt - so fallen unplausible Werte schon in der Übersicht auf, ohne
    dass man jede Riege einzeln öffnen muss
- **Harte Plausibilitätsgrenze beim Speichern**: zusätzlich zur rein
  optischen Warnung wird beim Speichern eines Ergebnisses serverseitig
  geprüft, ob der Wert überhaupt menschenmöglich ist
  - 0 oder negative Werte werden immer abgelehnt
  - Werte, die um ein Vielfaches außerhalb der obigen Grenzwerte liegen
    (z.B. 100 Sekunden bei einem 50m-Sprint), werden ebenfalls
    abgelehnt - ein lediglich sehr gutes oder sehr schwaches Ergebnis
    dagegen nicht
  - Eine abgelehnte Eingabe wird nicht gespeichert und stillschweigend
    verworfen - stattdessen erscheint eine deutliche Fehlermeldung, ein
    zuvor gespeicherter Wert bleibt dabei unverändert erhalten
- **Punkteberechnung**: Automatisch nach Eingabe
  - Basierend auf aktuellem Punktespiegel
  - Sofortige Anzeige der Punkte
- **Messmethode manuell/elektronisch (nur Sprint-Disziplinen)**: Pro
  Ergebnis auswählbar, ob die Zeit manuell oder elektronisch gestoppt
  wurde
  - Umschalter oben setzt die Messmethode für alle Zeilen einer Riege
    gleichzeitig, jede Zeile kann die Auswahl aber auch einzeln
    übersteuern
  - Vorausgewählt ist "manuell"; Änderungen werden sofort per Autosave
    gespeichert
  - Wirkt sich auf die Punkteberechnung aus (siehe "Punkteberechnung"
    unten) - Disziplinen-Matrix und Sprintweiten-Auswahl bleiben
    unverändert
  - **Eingabegenauigkeit folgt der Messmethode**: Die Nachkommastellen
    im Ergebnisfeld richten sich nach der in der Disziplinen-Matrix
    gewählten "Messgenauigkeit für Sprint" (Zehntel/Hundertstel), außer
    bei elektronischer Messung - dort werden unabhängig von dieser
    Einstellung immer Hundertstelsekunden verwendet, da eine
    elektronische Zeitmessung technisch bereits diese Genauigkeit
    liefert. Jede Zeile zeigt dadurch ihre eigene, zur individuellen
    Messmethode passende Eingabemaske. Wird eine Zeile von
    elektronischer auf manuelle Messung zurückgestellt, wird ein bereits
    auf Hundertstel erfasster Wert automatisch kaufmännisch auf
    Zehntelsekunden gerundet (z. B. 8,57 → 8,6)
- **Riege abschließen**: Markiert Riege als fertig erfasst
- **Tastaturbedienung**: Optimiert für schnelle Eingabe (Enter, Esc)

### 5. Punkteberechnung

- **Amtlicher Punktespiegel**: Aktuelle Tabelle der Bundesjugendspiele
  - Unterscheidung nach Wettkampfjahr (I/II)
  - Geschlechtsspezifische Tabellen
  - Altersklassen-spezifische Werte
- **Automatische Berechnung**: Punkte werden sofort nach Ergebniseingabe berechnet
- **Handzeit-Korrektur bei Sprint-Disziplinen**: Die amtliche Formel
  gleicht bei manueller Zeitmessung die systematisch spätere Reaktion
  beim Stoppen mit 0,24s aus - bei elektronischer Zeitmessung (siehe
  "Ergebniserfassung" oben) entfällt dieser Ausgleichswert vollständig
- **Gesamtpunktzahl**: Summe aller Disziplinen pro Teilnehmer
- **Urkunden-Einstufung**: Automatische Zuordnung zu Teilnahme/Sieger/Ehrenurkunde

### 6. Urkunden

- **PDF-Druck**: Individuelle Urkunden für jeden Teilnehmer
  - Teilnahmeurkunden: Für alle Teilnehmer
  - Siegerurkunden: Ab bestimmter Punktzahl (z.B. ≥ 625 Punkte)
  - Ehrenurkunden: Für besondere Leistungen (z.B. ≥ 825 Punkte)
- **Filterfunktion**: Urkunden nach Kriterien filtern
  - Nach Klasse: z.B. "05a"
  - Nach Jahrgangsstufe: z.B. "5" (alle 05a, 05b, 05c)
  - Nach Urkundenart: Teilnahme/Sieger/Ehren
- **Massendruck**: Alle gefilterten Urkunden in einem PDF
- **Anpassbares Layout**: Positionen der Urkundenfelder konfigurierbar

### 7. Ergebnisbericht

- **Übersicht**: Alle Ergebnisse aller Teilnehmer
  - Nach Klasse sortiert
  - Mit Gesamtpunktzahl
  - Mit Urkundentyp
- **Export**: Ergebnisse als CSV exportierbar
- **Statistik**: Übersicht nach Klassen, Jahrgängen, Disziplinen

### 8. Dashboard

- **Kennzahlen auf einen Blick**: Teilnehmer gesamt, abgeschlossene Riegen,
  erfasste Ergebnisse, eingestufte Urkunden - jeweils mit Prozentanteil
- **Fortschrittsbalken**: Riegen-Status (offen/teilweise/abgeschlossen)
- **Urkunden-Verteilung gesamt**: Ehren-/Sieger-/Teilnehmerurkunden als
  gestapelter Balken
- **Klassenvergleich**: ein Balken je Klasse zeigt die Verteilung
  innerhalb der Klasse - Unterschiede zwischen Klassen auf einen Blick
- **Tabellenansicht**: alle Zahlen zusätzlich exakt und aufklappbar

## Verwendung

### Workflow

1. **Teilnehmer importieren**: CSV-Datei mit Stammdaten aus der Schulverwaltung hochladen
2. **Disziplinen anlegen**: Sprint, Sprung, Wurf konfigurieren
3. **Riegen bilden**: Automatische Gruppierung nach Kriterien
4. **Wettkampf durchführen**: Ergebnisse pro Riege erfassen
5. **Urkunden drucken**: PDF mit Filter nach Klasse/Jahrgang erstellen

### CSV-Format

Die Teilnehmer-CSV-Datei muss folgende Spalten enthalten:

| Spalte | Beschreibung | Beispiel |
|--------|--------------|----------|
| Vorname | Vorname des Teilnehmers | Max |
| Nachname | Nachname des Teilnehmers | Mustermann |
| Klasse | Klassenbezeichnung | 05a |
| Geschlecht | m = männlich, w = weiblich | m |
| Geburtsjahr | Vierstellige Jahreszahl | 2016 |

**Beispiel-CSV**:
```csv
Vorname,Nachname,Klasse,Geschlecht,Geburtsjahr
Max,Mustermann,05a,m,2016
Erika,Musterfrau,05a,w,2016
```

## Lizenz

Dieses Projekt ist für den internen Gebrauch an Schulen gedacht.

**Viel Erfolg bei den Bundesjugendspielen!** 🏆
