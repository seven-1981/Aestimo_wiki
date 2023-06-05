# Anforderungsquellen

Die Anforderungen an das Tool kommen aus den folgenden drei Bereichen
- Personen und Organisation
- **Dokumente**
- Systeme im Betrieb

Dieses Kapitel befasst sich mit den Dokumenten, welche Einfluss auf die Anforderungen an das Tool Aestimo nehmen. 

# Bahnnorm IEC 61508

## Vorgehensmodell
Die Firma Schweizer Electronic AG, welche im sicherheitsgerichteten Umfeld funktional sichere Geräte für Warnsysteme an der befahrenen Schiene entwickelt und vertreibt, hält sich bei der Entwicklung an die Vorgaben der Bahnnorm IEC 61508. Diese schlägt als Vorgehensmodell das V-Modell vor. Die folgende Abbildung zeigt das V-Modell, wie es die Firma Schweizer Electronic AG ausgelegt hat. 

![VModell.PNG](/.attachments/VModell-1bbb3cce-9a28-4ffc-a6de-eb30911e5127.PNG)

## Bisheriger Prozess des Code Walk Through
Die Implementierung von Software wird in der Phase 6.4 realisiert. Als Teil der Phase 6.4 wird ein Code-Walk-Through, kurz CWT, über den gesamten Source Code gemacht. Die Software, welche auf den Geräten der Schweizer Electronic AG eingesetzt wird, wird gemäss Norm in Software Komponenten gegliedert. Beim Code Walk Through wird pro Software Komponente jedes Source File von einem qualifizierten Software-Entwickler, welcher nicht der Implementierer des Codes ist, ge-reviewed. Die Review Befunde werden in einer Excel Tabelle erfasst. Am Ende des Reviews werden die Befunde in einem Review-Meeting, in welchem auch der Implementierer des Source Codes dabei sein muss, diskutiert und bewertet. Kritische Befunde müssen im Nachgang vom Implementierer korrigiert werden und es folgt ein erneutes CWT des geänderten Source Files. Diese Prozedur wird solange wiederholt, bis es keine Review-Befunde mehr gibt.   

## Kritikpunkte am bisherigen Prozess
## Qualität der Reviews
Die Erfahrung zeigt, dass die Qualität eines Reviews mit zunehmender Dauer abnimmt. Bei den Software Komponenten kann es sich um tausende von Zeilen Sourcecode handeln. Oftmals werden die Reviews in einem Stück durchgeführt, sodass je länger das Review dauert die Codezeilen nur noch überflogen werden und kritische Stellen übersehen werden. 

## Doppelte Arbeit
Die Bahnnorm EN50128, eine der IEC 61508 untergeordnete Norm, verlangt das 4 Augen-Prinzip. Jeder Entwicklungsschritt muss verifiziert werden, so auch jede neu entstandene oder geänderte Zeile Code. Das Software-Entwicklungsteam verfiziert den Sourcecode über Peer Reviews bei Pull Requests. Die daraus entstehenden Befunde bezehen sich jeweils auf ein paar wenige Sourcefiles und sind qualitativ hochstehend. Das Software-Team und somit auch der Reviewer sind während der Entwicklung in Phase 6.4 viel tiefer mit der Thematik des neuen Features vertraut als am Ende der Implementierungsphase 6.4. Der Prozess fordert am Ende dieser Phase 6.4 sozusagen den Beleg, dass das 4 Augen Prinzip eingehalten wurde. Der Code Walk Through ist dieser Beleg. Wie dieser Report zustande kommt liegt in der Verantwortung der Entwicklung. Bisher wurden in der Software-Entwicklung die Peer Reviews durchgeführt und zusätzlich noch das Code Walk Through. Die Arbeit wird also doppelt verrichtet. 

## Falsche Anwendung eines Code Walk Throughs
Der Code Walkthrough ist eine Form von Peer Review mit einem oder mehreren Mitgliedern, bei welcher der Implementierer den Review-Prozess leitet und die anderen Teammitglieder Fragen zum Sourcecode stellen und mögliche Fehler im Vergleich zu den Entwicklungsstandards und andere Probleme erkennen sollen. 
Das im Kapitel [Bisheriger Prozess des Code Walk Through](Bisheriger Prozess des Code Walk Through) beschriebene Vorgehen weicht deutlich von dieser Definition ab. Die Peer Reviews bei der Feature-Implementierung sind zwar offiziell auch keine CWTs, können aber bei Bedarf auf solche hochgestuft werden, wenn es sich zum Beispiel um eine sehr komplexe Änderung handelt. Der Implementierer ist zu diese Zeitpunkt auch voll im Bilde und kann jede Zeile Code rechtfertigen. Am Ende der Implementierungsphase kann er sich möglicherweise schon nicht mehr an alle seine Zeilen Code erinnern. 
