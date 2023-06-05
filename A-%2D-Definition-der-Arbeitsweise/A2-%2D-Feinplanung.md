# Organisation
Für die Feinplanung wird ein an Scrum angelehntes Vorgehensmodell gewählt, welches in der Folge beschrieben wird. Vorweg gilt zu erwähnen, dass dabei nicht alle Scrum Events regelmässig durchgeführt werden resp. da es sich um ein zweier Team handelt werden auch die Scrum Rollen nicht eingehalten. Beide Teammitglieder versetzen sich in jede Rolle.

Es werden "Sprints" im 2 Wochen Takt durchgeführt. Von Beginn des Projekts bis zur Abgabe der Arbeit sind dies 11 Sprints resp. bis zur Abschlusspräsentation 12 Sprints. Zu Beginn jedes Sprints werden innerhalb einer Sitzung im Team ein Backlog Grooming sowie eine Sprintplanung durchgeführt, um die Aufgaben für die nächsten zwei Wochen zu planen. Die Inhalte einer Retrospective können in diesem Meeting ebenfalls Platz finden. Die Sprintplanung soll jeweils innerhalb von 2 Stunden durchgeführt werden.
Es gibt kein Daily Stand-up, jedoch stehen die Teammitglieder innerhalb eines Sprints in regelmässigem Austausch, je nach Bedarf mehr oder weniger. Es sind keine offiziellen Sprint Reviews vorgesehen, jedoch ist die Idee, dass das Team jeweils nach jedem Sprint kurz den Projektfortschritt der Betreuungsperson aufzeigt und bei Bedarf diskutiert und Inputs mit in die weitere Planung einfliessen lässt.
Der Aufwand der Tasks wird in Manntagen geschätzt.

# Tool
Für die Anwendung der Sprints wird das Azure DevOps verwendet, welches gleichzeitig auch als Code-Hosting Tool und Continuous Integration Plattform sowie zu Dokumentationszwecken verwendet wird. Es wird nicht das Ziel verfolgt, alle Möglichkeiten von Azure DevOps auszuschöpfen, sondern das Tool für unsere Anwendung zielführend einzusetzen. Darunter fällt auch, dass nicht alle Tasks bis ins letzte Detail ausdefiniert sind wie z.B. werden gewisse Felder nicht verwendet. 

Die Arbeitspakte werden nach folgendem Muster behandelt:
![image.png](/.attachments/image-fdcd9729-c930-4555-aa98-a27e6b9ed4b4.png)
Quelle: Microsoft Azure DevOps Dokumentation:
https://learn.microsoft.com/en-us/azure/devops/?view=azure-devops

Die Epics unseres Projekts entsprechen der Themenbereiche der [Grobplanung](https://dev.azure.com/michaelbos0816/Aestimo/_wiki?pageId=3&friendlyName=Grobplanung#). Die Features repräsentieren die tiefer gelegende Ebene, sie sind in der [Grobplanung](https://dev.azure.com/michaelbos0816/Aestimo/_wiki?pageId=3&friendlyName=Grobplanung#) als Arbeitspakete betitelt und jeweils mit einer zweistelligen Ziffer identifiziert. Für die Feinplanung mittels Sprints werden die Features weiter aufgegliedert in User Stories resp. Bug Stories, welche für die Sprints wiederum in Tasks aufgeteilt werden. 
Auf Issues wird für dieses Projekt verzichtet. 

Bei diesem Vorgehen erfassen wir auch Arbeiten als User Stories, die nicht direkt aus einer Anforderung an die Software entstehen. Es gibt folglich auch User Stories für allgemeine Arbeiten wie zum Beispiel die Vorbereitung auf das Zwischenreview, etc. Das bietet für uns den Vorteil, dass wir alle "ToDos" eines Sprints im Sprint Backlog abgebildet haben und die Gefahr, dass eine Arbeit vergessen geht, reduzieren. 

# Sprint Backlog Workflow
Die Tasks können 4 verschiedene Zustände haben; To Do, In Progress, Verification und Done. Wenn ein Task von einem Teammitglied bearbeitet und fertiggestellt wurde, wandert dieser Task auf Verfication. Das bedeutet, dass das andere Teammitglied die Arbeit reviewen muss, der Task wandert also erst auf Done, wenn er zuvor verifiziert wurde. Beim Review geht es zum einen um den Informationsaustausch und Transparenz und zum anderen, die Arbeit auf Fehler, Mängel und Optimierungen zu prüfen. 



