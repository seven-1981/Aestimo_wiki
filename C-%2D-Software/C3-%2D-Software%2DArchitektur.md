# Architektur Aestimo

## Grobe Übersicht (Flow-Diagramm) Version 1

![image.png](/.attachments/image-6a106341-cc48-4f45-bea8-0501b1224f28.png)

## Version 2
Befund Michael: Es fehlten die Review-Befunde, Auch Atlassian kann das API in der Cloud ändern
![image.png](/.attachments/image-40b58975-8912-4f53-bc1c-4d652f259933.png)

Im Prinzip liessen sich die Flows "Befunde und Kommentare" ebenfalls über dasselbe REST API eintragen, dies ist sogar eine der möglichen Erweiterungen des Tools. Die "Basisversion" aber kann lediglich Kommentare abfragen und keine Reviews machen und Kommentare eintragen (nur GET, kein PUT oder POST), daher ist es ihr egal, wie die Befunde eingetragen werden.

## Erste Überlegungen zur SW-Architektur

Die logischen funktionalen Einheiten von Aestimo lassen sich nach dem bisherigen Erkenntnisstand wie Folgt einteilen :
- Präsentation / Bediener-Interaktion : Eine Einheit dient dazu, die Eingaben vom Benutzer des Tools entgegenzunehmen und Informationen anzuzeigen.
- Business-Logic : Eine Einheit erledigt die Verarbeitung der Eingaben vom Bediener und von den vom Bitbucket-Server empfangenen Daten.
- Server-Anbindung / REST API : Eine weitere Einheit dient dazu, die Daten vom Bitbucket-Server abzufragen. Diese Einheit könnte auch als Persistenz-Einheit betrachtet werden, mit dem Unterschied, dass die Daten nicht durch Aestimo selbst, sondern durch einen Cloud-Dienst verwaltet werden.

Die obigen Überlegungen legen nahe, für Aestimo eine Drei-Schichten-Architektur anzustreben. Dadurch lassen sich die funktionalen Belange unabhängig voneinander entwickeln und später warten resp. updaten. 

Die SW-Architektur soll also als Schichtenmodell umgesetzt werden :

|Name der Schicht |Namespace / Verzeichnis |Beschreibung  |
|----------------------|--|--|
|User Interface Layer     |UserInterface / uil  |Eingaben des Bedieners, Anzeigen der Informationen, Ausgabe der generierten Reports.   |
|Business Logic Layer     |BusinessLogic / bll  |Verarbeitung der empfangenen Befehle, erstellen eines Reportes aus den empfangenen Daten. |
|Data Access Layer        |DataAccess    / dal  |Zugriff über REST API auf Bitbucket-Server und Rückgabe der Objekte gemäss API Modell.     |

Die SW soll so designt werden, dass
- Das Schichtenprinzip eingehalten wird, d.h. dass der Zugriff nur von "oben nach unten" geschieht, d.h. dass keine Schicht auf die darüberliegende Schicht zugreifen darf.
- Die Komponenten innerhalb der Schicht sollen soweit möglich unabhängig voneinander sein (keine grösseren "Quer-Abhängigkeiten"). Dies soll die Austauschbarkeit resp. die Erweiterbarkeit gewährleisten.
- Funktionale Einheiten, die an vielen Stellen gebraucht werden, sollen in eigene Komponenten und in einer "Utility" Komponente untergebracht werden. "Utilities" dürfen keine Abhängigkeiten zur Applikation haben.
- Cross Cutting Concerns (z.B. Logging) sollen ebenfalls abstrahiert werden, d.h. mindestens durch eine Komponente gewrappt werden, so dass ein späterer Austausch möglich ist (z.B. Wechsel des Logging-Frameworks).

## Data Access Layer

Im Working Skeletton wurde der Data Access Layer wie Folgt implementiert:
- Grundsätzlich besteht der Layer aus zwei Varianten, Server und Cloud (im UML-Diagramm ist nur ein Pfad dargestellt). 
- HTTP GET Requests werden von der RESTApiClient Klasse getätigt. Diese erzeugt mit dem Refit Framework das Objekt zum IRESTApiService (nur Interface ist bekannt).
- Der HTTP Client wird dem Service per Dependency Injection mitgegeben, dort werden auch die eigenen HTTP Message Handler Klassen mitgegeben für das Logging und das Error Handling (Delegating Pattern).
- Das Interface, was dem Business Logic Layer zur Verfügung gestellt wird, ist ähnlich zum IRESTApiService, aber heisst IRESTApiClient. Dieses wird dann von den Service Adaptern auf dem Service Adapter Layer verwendet.

![image.png](/.attachments/image-0003b893-8d5c-4622-80ca-ac39cb491b59.png)

## Nächste Iteration SW-Architektur

Am zyklischen Meeting vom 10.05.2023 mit Martin gab es einige wertvolle Inputs, die einen wesentlichen Impact auf die SW-Architektur, resp. die ganze Projektstruktur hatten.
Das vorgeschlagene Layer-Modell ist OK. Es wäre aber besser, die Layer nicht als Verzeichnisse in einem Projekt in der Solution, sondern als eigene Projekte zu verwalten. Zudem wäre ein eigener Layer für die Service Adapter ebenfalls eine mögliche Variante. Dadurch lässt sich die Schichtentrennung viel besser und gezielter durchsetzen und überprüfen (per "Project References" im Visual Studio). Zudem lässt sich so relativ einfach ein zusätzliches Projekt als Konsolenapplikation über die unterliegenden Schichten setzen.

Das Flow Diagramm Version 2 ist immer noch korrekt, wenn man sich die beiden Blöcke "REST API" als eigenständigen Layer vorstellt.

Dies wurde im Visual Studio so umgesetzt. Es gibt neu also die folgenden Layer:
![image.png](/.attachments/image-a18b933b-b44d-4ad4-a93a-1821dafe5103.png)

Die 4 Layer setzen die strikte Schichtentrennung um, d.h. Zugriff erfolgt immer von "oben nach unten". Die beiden Model-Projekte werden von mehreren Schichten verwendet. Diese könnten zwar auch in die Layer integriert werden, aber die Übersicht ist so besser und allenfalls kann dies später bei Bedarf immer noch vereinfacht werden.

Zusätzlich gibt es ein Utility-Projekt, auf welches von überall zugegriffen werden darf (Cross-Cutting, Hilfsklassen, Logging, etc.).

Die Projekte werden, abgesehen vom User Interface Layer, welcher als WPF Projekt konfiguriert ist, als einfache Class-Libraries konfiguriert. 

|Name der Schicht |Projekt / Namespace |Beschreibung  |
|----------------------|--|--|
|User Interface Layer     |Aestimo / uil  |Eingaben des Bedieners, Anzeigen der Informationen, Ausgabe der generierten Reports.   |
|Business Logic Layer     |BusinessLogic / bll  |Verarbeitung der empfangenen Befehle, erstellen eines Reportes aus den empfangenen Daten. |
|Service Adapter Layer    |ServiceAdapter / sal  |Abkapselung der unterschiedlichen REST API, zusammenführen auf ein einzelnes Service-Adapter Interface     |
|Data Access Layer        |DataAccess    / dal  |Zugriff über REST API auf Bitbucket-Server und Rückgabe der Objekte gemäss API Modell.     |

Zwei weitere Projekte fungieren als "Model-Projekte" und dienen für die Ablage der Model-Klassen. 
|Name |Projekt / Namespace |Beschreibung  |
|----------------------|--|--|
|Business Model     |BusinessModel / bmo  |Business Model, von uns vorgegebene Klassen, die die verwendeten Entitäten (wie Pull-Requests, Commits, Reports, Befunde, etc.) modellieren.   |
|Data Access Model     |DataAccessModel / dmo  |REST API Modell, von Bitbucket vorgegeben (auch Klassennamen, Attribute etc.). |