# Anforderungsquellen

Die Anforderungen an das Tool kommen aus den folgenden drei Quellen
- **Personen und Organisation**
- Dokumente
- Systeme im Betrieb

Dieses Kapitel befasst sich mit den Personen und der Organisation, also Personen, die direkt oder indirekt Einfluss auf die Anforderungen an das Tool Aestimo haben. 

# Nutzungskontext
An dieser Stelle wird kurz der Nutzungskontext des Tools Aestimo anhand der wichtigsten Eigentschaften, welche die Stakeholder mitbringen sollen, aufgeführt. Die Punkte beziehen sich jeweils auf alle Stakeholder, sprich diejenigen, welche das Tool anwenden bzw. verwalten und diejenigen, welche mit dessen Output arbeiten. 

## Menschliche Faktoren: Firmenwissen und Firmenkultur
Die Stakeholder müssen den Entwicklungsprozess der Entwicklungsabteilung kennen. Des Weiteren müssen sie den Prozess einer Software-Release Erstellung kennen. Ansonsten wird kein weitere Wissen über die Firma und deren Kultur vorausgesetzt. 

##  Inhaltliche Faktoren: Normen
Die Bahnnorm IEC 61508 und deren Unternormen geben den Entwicklungsprozess vor.
Die Stakeholder müssen sich der Normen bewusst sein, unter welchen die Software entwickelt wird und welche auch das Code Walk Through in der entsprechenden Entwicklungsphase vorgibt. 
    
## Organisation: Altsysteme
Die Stakeholder müssen Kenntnisse über die älteren Projekte verfügen, welche unter dem selben Entwicklungsprozess realisiert wurden. 

## Gruppendynamik 
Die Entwicklung arbeitet in Teams, deren Mitglieder eng miteinander zusammenarbeiten und grundsätzlich alle wichtigen, technischen Entscheidungen auch im Team trifft. Gruppendynamik ist somit erwünscht. 
   
## Motivierte Stakeholder mit hohem Abstraktionsvermögen 
Das derzeitige Team besteht aus jungen, motivierten und belastbaren Mitarbeitern. Eine technische, akademische Ausbildung ist Anstellungsbedingung. Somit kann von den Stakeholdern ein hohes Abstraktionsvermögen vorausgesetzt werden, obwohl die Anwendung des Tools ein solches nicht verlangt. Die Komplexität der Anwendung ist überschaubar. 

## Geringes Machtgefälle der Stakeholder
Die Stakeholder des Tools weisen ein geringes Machtgefälle auf. Neben einem Scrum-Team, in welchem alle Mitarbeiter grundsätzlich gleichberechtigt sind, gibt es den Project Manager, welcher der direkte Vorgesetzte des Teams ist.

## Verfügbarkeit der Stakeholder
Die Stakeholder, welche in Frage kommen, arbeiten alle am gleichen Standort und ungefähr zu den gleichen Arbeitszeiten. Da diese Masterarbeit jedoch nicht während der Arbeitszeit behandelt werden kann, müssen die Stakeholder ausserhalb der Arbeitszeiten verfügbar sein.     


# Personas
## Software-Entwickler
Der Software-Entwickler ist diejenige Person, welche das Wissen über die Software-Releases besitzt und anhand dessen die Reports generiert. Die digitale Unterschrift des Software-Entwicklers wird im Report hinterlegt, er ist der Autor des Dokuments. 
Ziel des Entwicklers ist es, ein qualitativ möglichst hochstehendes Review des Source Codes durchzuführen und dieses sauber dokumentiert zu haben. 

## Project Manager
Der Project Manager ist diejenige Person, welche den Report als aktives Dokument freigibt. Dazu wird seine digitale Unterschrift im Report hinterlegt. 
Ziel des Project Managers ist es, einen TüV fertigen Code Walk Through Report zu haben, welcher in möglichst kurzer Zeit realisiert werden kann. 

## Administrator Bitbucket Server 
Der Administrator besitzt die Rechte, den Bitbucket-Server zu modifizieren. 

## Validierer
Der Validierer hat, bezogen auf den freigegebenen Report, die Aufgabe, den Inhalt zu validieren. 
Ziel des Validierers ist es, einen verständlichen und nachvollziehbaren Code Walk Through Report vorzufinden, welcher möglichst alle Schwachstellen im Code aufgedeckt und zur Umsetzung derer geführt hat.

## TüV
Der technische Überwachungsverein zertifiziert die entwickelten Produkte. Dazu prüft er, ob sich die Firma Schweizer Electronic AG an den von der Norm vorgegebenen Entwicklungsprozess gehalten hat und tut dies anhand der Dokumentation. Der CWT Report fällt auch unter diese Dokumentation. 
Der TüV ist nur ein indirekter Stakeholder, da er grundsätzlich lediglich die Normen vertritt. Zudem gelangt der Output des Tools Aestimo zuerst in die Hände des Validierers und des Project Managers, bevor er zum TüV gelangt. Aus diesem Grund wird eine Anforderungserhebung mit dem TüV nicht in Betracht gezogen. 


# Stakeholder-Tabelle
Für die Erhebung der Anforderungen sollen die folgenden Stakeholder der Firma Schweizer Electronic AG befragt werden:


|Name  |Persona |Fokus  | 
|--|--|--|
| Peter Hürzeler | Project Manager / Administrator| Gibt die TüV relevanten Arbeitsdokumentationen frei/ Verwaltet den Bitbucket-Server |
| Daniel Habermacher| Validierer| Validiert die TüV relevanten Arbeitsdokumentationen |
| Simon Theiler | Software-Entwickler Embedded | Erstellt die TüV relevanten Arbeitsdokumentationen  |
| Pascal Häfliger | Software-Entwickler Embedded  | Erstellt die TüV relevanten Arbeitsdokumentationen |



