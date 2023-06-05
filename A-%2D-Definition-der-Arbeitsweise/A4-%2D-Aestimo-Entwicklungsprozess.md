# Beschreibung der Entwicklungsumgebung

Da Aestimo eine in C# geschrieben WPF Applikation sein wird liegt es nahe, als Entwicklungsumgebung auf Visual Studio zu setzen. Es wird die Visual Studio 2022 Community Edition, welche direkt auf den Studien-Laptops installiert wird, verwendet. Als Betriebssystem wird Windows 10 64bit eingesetzt.

Als Programmiersprache kommt C# zum Einsatz. Die Anwendung wird in .NET 6 geschrieben (Long Term Support).

Das GUI soll mit dem WPF (Windows Presentation Foundation) Framework umgesetzt werden. Damit ist die Laufzeitumgebung ohne weitere Massnahmen auf Windows beschränkt. Es gäbe die Möglichkeit, WPF auf einer Linux-Umgebung laufen zu lassen, doch dafür müsste man sich z.B. 'Wine' Anschauen: https://ccifra.github.io/PortingWPFAppsToLinux/Overview.html.

## Verwendete Tools

Als zusätzliches Tool wird Fine Code Coverage als Plugin in der Entwicklungsumgebung installiert.


# Beschreibung des Entwicklungsprozesses

## Implementation

### Richtlinien

Die "best practices" für C# und .NET sollen eingehalten werden. Insbesondere wird auch auf einen einheitlichen Codierstil geachtet. Siehe dazu die Seite von [Microsoft](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions).

### Tool-Unterstützung, statische Codeanalyse

Bei der Implementation soll die Unterstützung der Entwicklungsumgebung möglichst ausgenutzt werden, das heisst, dass die automatisch laufende, statische Codeanalyse beachtet werden soll. Es hat sich bewährt, die Massnahmen gemäss einer Abstufung nach Schwere der Meldungen zu wählen :
  - Fehler müssen zwingend korrigiert werden.
  - Warnungen müssen ebenfalls korrigiert werden. In Ausnahmefällen können sie ignoriert werden, müssen dann allerdings im Code mit einer stichhaltigen Begründung versehen werden.
  - Infos / Meldungen dürfen zwar ignoriert werden, sollen aber nach Möglichkeit ebenfalls korrigiert werden.

### Tests

Weiter sollen nach Möglichkeit immer während der Implementation (im Idealfall vor dem Implementieren des Codes) Unit Tests geschrieben werden (Test Driven Development). Die Codeabdeckung soll dabei regelmässig mit dem Fine Code Coverage Plugin analysiert werden und die Hotspots (d.h. Stellen im Code mit keiner oder schlechter Abdeckung) sollen identifiziert werden, damit dann gezielt dafür Tests geschrieben werden können.

Die Tests mit dem REST API sollen von den Unit Tests separiert werden und als Integrationstest in einem eigenen Projekt in derselben Solution untergebracht werden.

### Umgang mit TODO und HACK

Nach Möglichkeit sollen keine unvollständigen Implementationen auf das Azure DevOps Repository gepusht werden. Ist dies unumgänglich, wird der Code mit einem "//TODO" Kommentar versehen. Der Kommentar soll in kurzer Form erklären, was genau noch fehlt resp. noch zu implementieren ist.

Ist für eine Implementation keine "elegante" oder "kurze" Lösung bekannt, kann der Code mit einem Kommentar "//HACK" versehen werden. Der Code muss korrekt sein, aber kann dann zu einem späteren Zeitpunkt nochmal richtig implementiert werden. So soll die Möglichkeit geschaffen werden, dass man die Implementation weiter vorantreiben kann und nicht durch die Stelle aufgehalten wird.

Achtung! Kein Abstand zwischen '//' und 'TODO' resp. 'HACK', sonst wird die Stelle von VS2022 nicht erkannt! In VS2022 kann die Liste im Menu "View, Task List" gefunden werden.

## Source Code Management und Code Hosting

Da als Source Code Management (SCM) Tool in der Firma 'git' zum Einsatz kommt und dessen Grundlagen und Anwendung gut bekannt sind, bietet es sich an, dieses auch für Aestimo einzusetzen. Der Code wird auf einem Repository auf Azure DevOps gehostet.

## Git Workflow

Für die Implementation wird weitestgehend der auf der [Wiki Seite zum Git Workflow](https://dev.azure.com/michaelbos0816/Aestimo/_wiki/wikis/Aestimo.wiki/18/D1-Git-Workflow-SEAG) beschriebene Arbeitsprozess eingesetzt. Entwickelt wird auf Feature Branches, die zyklisch in einen Entwicklungsbranch zurück gemerged werden. Bei einem Release wird der Entwicklungsbranch in den Master Branch zurückgeführt und auf diesem wird dann ein Tag erstellt.

Die Branches werden entsprechend den Namen der Tasks auf dem Sprint-Board benannt, so dass Azure DevOps einen automatischen Link herstellen kann und so dass klar ist, zu welchem Task ein Branch gehört. Es sollen keine Arbeiten ohne dazugehörige Tasks im Repository gemacht werden.

Die Branch Policies werden so eingestellt, dass
- auf dem Master Branch und dem Entwicklungsbranch keine direkten Commits möglich sind. Anpassungen sind nur über Pull Requests möglich, wenn ein Feature Branch in den Entwicklungsbranch zurückgeführt wird.
- jeder Pull Request vom Partner einem Peer-Review unterzogen werden muss, bevor dieser gemerged werden darf. Es dürfen nur Pull Requests gemerged werden, bei denen alle Befunde (Kommentare) aufgelöst sind, d.h. im gegenseitigen Einverständnis.

Dieses Vorgehen hat sich bewährt, da durch das Vieraugenprinzip die Qualität des Codes erhöht wird.

Bei Bedarf (dies wird zyklisch an den Sprint Events, resp. während des Tages bei der Arbeit besprochen) kann auch ein Pair-Programming für eine abgeschlossene Aufgabe zum Einsatz kommen. Auch dies hat sich in der Firma bewährt, da man zwei Fliegen mit einer Klappe schlägt :
- das Know-How wird automatisch auf zwei Personen verteilt, zwei Personen haben mehr Ideen, sehen mehr als eine und
- es ist kein zusätzlicher Aufwand für das Peer-Review mehr nötig, da beide Personen den Code schon sehr gut kennen.

Die "best practices" zur Anwendung von git sollen eingehalten werden:
- Kleine, bedeutungsvolle Commits machen
- Nur lauffähige Commits (sonst z.B. 'stashes' verwenden)
- Jeder Commit erhält eine kurze Beschreibung, was genau geändert wurde und die Begründung dazu
- Nur auf das remote Repository pushen, wenn man sicher ist, dass die Pipeline auch durchläuft, d.h. vorher bei sich lokal die Applikation kompilieren und die Tests ausführen

## Continuous Integration

Auf der Azure DevOps Plattform wird eine Pipeline eingerichtet. Die Pipeline soll die Applikation und alle dazugehörenden Tests automatisch kompilieren, sowie die Tests automatisch ausführen. 

Die Pipeline wird so aufgesetzt, dass sie bei jedem Push und jedem Merge Commit auf dem Repository gestartet wird. Der Build-Status soll zyklisch überprüft werden. Bei einem Fehlschlag hat die Korrektur des Fehlers oberste Priorität vor allen anderen Arbeiten!

Wichtig ist, dass der "free Tier" nur 1800 Minuten pro Monat zur Verfügung hat. Das Startprojekt läuft in ca. 2-3min durch, aber es ist davon auszugehen, dass die "build time" im Laufe des Projektes noch ansteigt. Daher soll nur auf das remote Repository gepusht werden, wenn dies Sinn macht, und nicht einfach bei jedem Commit, oder um die Tests auszuführen. Dies muss lokal gemacht werden.

# Releaseprozess

Bei einem Build auf der Azure DevOps Pipeline soll automatisch ein Archiv mit den Binaries erzeugt werden. Die Build-Nummer ist im Dateinamen des Archives ersichtlich.

Weiteres ist noch zu definieren (Anforderungen sind noch nicht vollständig erhoben und ausgewertet).

# SW-Architektur

Die Softwarearchitektur, welche vom Schichtenmodell vorgegeben wird, ist strikte einzuhalten. Dies bedeutet, dass es keine Zugriffe von einem Layer auf einen übergeordneten Layer geben darf (Unabhängigkeit)! Müssen Informationen von tieferliegenden Schichten (z.B. Business Logic) auf höhere Schichten (z.B. User Interface) gesendet werden, sind gängige Pattern und Mechanismen einzusetzen (Stichworte hier sind : Callbacks, Events, Observer, MVVM, Data Binding, Property Changed Events).

# Fremdsoftware

Externe Libraries sollen ausschliesslich über den Nuget Package Manager integriert werden. Es soll kein Code "manuell" ins Projekt kopiert werden. Zudem sollte darauf geachtet werden, dass die Libraries 
- einigermassen weit verbreitet und somit eine genügend grosse "Community" haben (Foren, Feedback im Netz)
- noch weiter entwickelt oder wenigstens noch gepflegt werden
- gut dokumentiert sind.
