# Technische Dokumentation
Nachfolgend werden die technischen Bestandteile der Applikation vorgestellt.

## Ordnerstruktur
Innerhalb des src-Verzeichnisses liegen folgende Unterordner:
- core
- schema
- CSVImports

### core
Innerhalb des cores befinden sich Funktionalitäten, welche nicht einer **einzelnen** spezifischen CSV-Datei oder einer **einzelnen** Tabelle der Datenbank zuordnen lassen.
Dazu gehören die Verbindung zur Datenbank (**Connection**.**py**), das Erstellen der Datenbank inkl. dessen Schema (**CreateDatabaseSchema**.**py**).
Weiterhin gibt es Wrapper Funktionen, welche mehrere Funktionalitäten zusammenfassen.

**InsertCsvData**.**py** fügt alle Daten eines Jahres aus den CSV-Dateien in die Datenbank ein.
Die Funktion ruft wiederum die einzelnen Funktionen aus dem Ordner **CSVImports** aus.

**InsertMetadata**.**py** fügt die statischen Metadaten in die Datenbank ein.
Sie sollte nur einmal pro erstellte Datenbank ausgeführt werden, jedoch vor dem erstmaligen einfügen von CSV-Dateien, da ansonsten Fremdschlüsselbeziehungen nicht funktionieren.

**SQLHelper**.**py** stellt Funktionen bereit, welche für die Optimierung der Imports verwendet werden.
Die Funktion **getItemDictionary()** speichert alle Einträge der item Tabelle in einem Python-Dictionary und gibt dieses zurück. Dies ermöglicht einen effizienten Zugriff auf die GlobalId ohne separaten Datenbankzugriff.
Das gleiche nur für die Banken-Tabelle und den GlobalLEI_Code geschieht in der Funktion **getBanksDictionary()**.

### schema
Innerhalb des schema-ordners befinden sich in den jeweiligen Python-Dateien die Insert-Funktionen für die statischen Metadaten.
Diese werden einmalig beim Erstellen der Datenbank ausgeführt.
Die Dateinamen entsprechen weitestgehend den jeweiligen Tabellennamen der Datenbanken. Alle Dateien werden innerhalb der Datei **InsertMetadata**.**py** geladen und aufgerufen.

### CSVImports
Der Ordner **CSVImports** beinhaltet alle relevanten Funktionalitäten für den Import von CSV Dateien in die Datenbank.
In den Dateien wird in der folgenden Reihenfolge vorgegangen:
- CSV-Datei laden
- Zeilen scannen und relevante Felder extrahieren
- SQL Befehl ausführen und auftretende Fehler behandeln
- Statistik Daten erstellen und Ausgeben

#### Dateien
- **ImportBank**.**py** Importiert banks.csv
- **ImportDataDictionary**.**py** Importiert sdd.csv
- **ImportCountryRankExposure**.**py** Importiert tr_cre.csv
- **ImportMarketRisk**.**py** Importiert tr_mrk.csv
- **ImportOthers**.**py** Importiert tr_oth.csv
- **ImportSovereign**.**py** Importiert tr_sov.csv
- **Output**.**py**
Beinhaltet Funktionen für die einheitliche Ausgabe von Status, Fehlern und Zusammenfassungen in der Konsole/Jupyter Notebook.

## CSV Dateien

Die CSV Dateien der EBA der Jahre 2015 bis 2020 sind in der Datei data.zip zu finden.
Sie müssen vor Durchführung des Imports in das Root-Verzeichnis als Ordner 'data' entpackt werden.
(Bsp.: banks.csv muss für das Jahr 2020 wie folgt vorliegen: data\2020\banks.csv)


Für jedes Jahr gibt es folgende Dateien:
- **banks.csv**
Diese Datei muss manuell erstellt werden.
Hierzu muss das Bankslist-Sheet der Metadata Exceltabelle in eine CSV-Datei extrahiert werden. 
Beachte: Die CSV-Datei muss UTF-8 codiert und durch ";" getrennt vorliegen.
- **sdd.csv**
Diese Datei muss manuell erstellt werden.
Hierzu muss das einzige Sheet der Data Dictionary Exceltabelle in eine CSV-Datei extrahiert werden. 
Beachte: Die CSV-Datei muss UTF-8 codiert und durch ";" getrennt vorliegen.
- **tr_cre.csv**
Diese Datei enthält alle Daten zur Credit Risk und kann ohne Anpassung verwendet werden.
- **tr_mrk.csv**
Diese Datei enthält alle Daten zur Market Risk und kann ohne Anpassung verwendet werden.
- **tr_oth.csv**
Diese Datei enthält alle weiteren Daten und kann ohne Anpassung verwendet werden.
- **tr_sov.csv**
Diese Datei enthält alle Daten zur Sovereign Debt Exposure und kann ohne Anpassung verwendet werden.
