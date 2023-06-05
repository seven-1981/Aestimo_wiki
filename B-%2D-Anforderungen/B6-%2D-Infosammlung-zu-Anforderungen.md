Hier können stichwortartig schon bekannte Punkte aufgeführt werden, die dann ggf. als Anforderungen aufgenommen werden können.


# Anforderungen an das zu entwickelnde Tool

- Die Gegenüberstellung der Versionen
  - Version Tool
  - Version Dokument
  - Versionen API

  soll im Tool vom Bediener abgefragt werden können (Konfigurationsmanagement).

- Das Tool soll die Möglichkeit bieten, über eine Komponentenliste jede Source-Datei einer Komponente zuordnen zu können.

- Das Tool soll für die HTTP Requests einen Timeout Mechanismus haben. Tests haben gezeigt, dass mehrere aufeinanderfolgende Abfragen bei guter Internetverbindung bis ca. im Sekundentakt ablaufen können. Das heisst, dass es bei komplexeren Abfragen sinnvoll wäre, 
  - ggf. einen "Retransmit" Mechanismus einzubauen
  - einen maximalen Timeout für die HTTP Requests einzubauen
  - für den Benutzer eine Art Fortschrittsanzeige einzubauen.


# Anforderungen an das zu generierende Dokument

- Die SW-Version des Tools (Major.Minor.Build), sowie die Version des Dokuments selbst soll im Dokument enthalten sein. Die Dokumentenversion wird gemäss SEAG-Dokumentenlenkung inkrementiert, wenn sich die Struktur des Dokumentes ändert.
- Die Commit-Hashes der Branches (Commits), die den "Diff" für den Report "aufspannen", sollen im Dokument enthalten sein.

# Anforderungen an den Prozess
- Implementierer und Reviewer müssen Teil eines erlesenden Teams sein und die nötige Qualifikation mitbringen