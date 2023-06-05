# Beschreibung Workflow
Die folgende Abbildung illustriert den Git Workflow, wie er in der Firma Schweizer Electronic AG in dem Embedded SW-Team angewendet wird. 

<IMG  src="https://www.nicoespeon.com/static/git-flow-branching-model-86b9c1cb60d6bba0e2ec6867d021f728-e0740.jpg"  alt="https://www.nicoespeon.com/static/git-flow-branching-model-86b9c1cb60d6bba0e2ec6867d021f728-e0740.jpg"/>

Quelle: https://www.nicoespeon.com/en/2013/08/which-git-workflow-for-my-project/

## Master Branch
Der Master Branch enthält immer den Software-Stand des aktuellsten Software-Releases. Das bedeutet auch, der Master Branch enthält immer eine stabile und lauffähige Version. Es können keine Änderungen direkt in den Master Branch eingepflegt werden, diese kommen ausschliesslich über einen Release Branch in den Master Branch.
Software-Releases werden auf dem Master Branch in Form von Tags erstellt. Die Tags unterliegen einer klar definierten Versionierung. 

## Develop Branch
Der Develop Branch entsteht durch Abzweigen aus dem Master Branch nach einem Software-Release. Das Embedded Team der Firma Schweizer Electronic AG wendet den Prozess so an, dass der Develop Branch eine unendlche Lebzeit besitzt, sodass der Develop Branch schon zuvor bestand. Der develop-Branch enthält in der Regel immer den neusten Entwicklungsstand der Software. Auch auf dem Develop Branch werden nicht direkt Änderungen vorgenommen. Solche werden ausschliesslich über Feature Branches in den Develop Branch gemerged. Bei diesen Merges gibt es zuvor immer ein Peer-to-Peer Review.

## Feature Branches
Feature Branches entstehen jeweils aus dem develop Branch. Auf ihnen werden neue Features implementiert, sie werden anschliessend wieder zurück in den develop Branch gemerged. In der Regel arbeitet jeder Software-Entwickler auf einem separaten Feature-Branch. 

## Release Branches
Sobald der Develop Branch die neuen Features drin hat, welche für ein nächstes Release eingeplant sind, wird aus dem Develop Branch ein Release Branch abgezweigt. Auf dem Release Branch werden während seiner Lebzeit ausschliesslich Bugfixes eingepflegt. Diese werden in Bugfix-Branches umgesetzt. Ein Bugfix Branch wird vom Release Branch abgezweigt, der Fehler wird behoben, und anschliessend wird der Bugfix Branch zurück in den Release Branch gemerged. Beim Mergen eines Bugfixes wird immer ein Peer-to-Peer Review durchgeführt. 

Ein Release Branch wird am Ende seiner Lebzeit zuerst in den Master Branch und anschliessend in den Develop Branch zurück gemerged. Bei diesen beiden Merges werden die Code-Änderungen zwar auch nochmals kurz stichprobenmässig überprüft, die Änderungen waren zu diesem Zeitpunkt aber sicher bereits bei einem Peer-to-Peer Review geprüft worden. 
Durch den Merge in den Master Branch sind auf dem Master die neusten Features und Bugfixes drin und es kann ein neues Tag erstellt werden. Durch das Zurückmergen in den Develop Branch sind auch die Bugfixes im Develop Branch drin. 

## Hotfix Branches
Im Falle eines Software-Bugs, welcher auf einem bestehenden Release detektiert wurde und welcher möglichst schnell behoben werden muss, kommen Hotfix Branches zum Einsatz. Diese werden vom Master Branch abgezweigt. Auf dem Hotfix Branch wird der Fehler gefixt und anschliessend wird ein Merge zurück in den Master gemacht. Bei diesem Merge wird immer ein Peer-to-Peer Review durchgeführt. Anschliessend kann auf dem Master ein neues Release erstellt werden, welches den Hotfix mit drin hat. 

## Erweiterung des Git Workflows
Grundsätzlich wird der oben beschriebene Git Workflow im Embedded Software-Team der Firma Schweizer Electronic AG eingehalten. Trotzdem gibt es Entwicklungsphasen, in welchen es Sinn macht, neben dem originalen Develop Branch einen weiteren "Entwickluns-Branch" zu haben. Ein solcher kommt zum Beispiel dann zum Einsatz, wenn die Software eine grundsätzliche Architekturänderung benötigt, was nicht mit einer überschaubaren Anzahl an Commits umsetzbar ist. Um in diesem Fall immer einen konsistenten Develop Branch zu haben, auf welchem parallel weiterentwickelt werden kann,  werden auf einem weiteren Entwicklungs-Branch , für die klare Unterscheidung wird auf dieser Seite im Weiteren vom Architektur Branch gesprochen, Änderungen über Feature Branches eingepflegt. Auch hier wird bei jedem Merge ein Peer-to-Peer Review durchgeführt. Am Ende der Lebzeit des Architektur Branches wird dieser wieder zurück in den Develop Branch gemerged, wobei es wiedernum nur ein stichprobenmässiges Review benötigt, da die Änderungen bereits ge-reviewed wurden. 


# Resultierende Anforderungen an das Tool Aestimo
Das Tool soll alle Befunde von Peer-to-Peer Reviews zwischen zwei Software-Releases erfassen können. Für den Report sollen nur Befunde von gemergten Pull Requests erfasst werden. Offene, abgelehnte und ersetzte Pull Requests sollen nicht berücksichtigt werden. 
Aus oben beschriebenem Workflow ergeben sich folglich Peer-to-Peer Reviews bei folgenden Fällen:

1. Mergen eines Feature Branches in den Develop Branch
1. Mergen eines Bugfix Branches in den Release Branch
1. Mergen eines Hotfix Branches in den Master Branch
1. Mergen eines Feature Branches in den Architektur Branch (o.ä. Zweiter Develop Branch)

## Normalfall
Die Punkte 1. und 2. machen den Normalfall aus. Hier muss das Tool in der Lage sein, zwischen zwei Software-Tags alles Befunde zu erfassen, welche bei Pull Requests in den Develop Branch und in den Release Branch zusammen gekommen sind. Eine Schwierigkeit dieses Szenarios ist es, den Start richtig zu erfassen. Da der Develop Branch nicht erst nach dem Software Release entsteht, muss der Zeitpunkt des Releases bestimmt werden können. Von diesem Zeitpunkt an müssen die Pull Requests auf dem Develop Branch erfasst werden können

## Hotfix Fall
In diesem Szenario gibt es zwischen zwei Software Releases lediglich ein oder mehrere Hotfix Branches. Hier ist die Schwierigkeit, dass die Pull Requests angezogen werden müssen, welche direkt in den Master gemacht werden (Punkt 3). Dieser Fall unterscheidet sich in Sachen relevante Branches somit vom Normalfall. Das Tool muss in der Lage sein, diese Fälle zu unterscheiden und die richtigen Pull Requests auszuwerten. 

## Spezialfall Architektur Branch
Bei der Verwendung eines Architektur Branches müssen auch diese Pull Requests in Betracht gezogen werden, falls diese Änderungen auch Bestandteil des Software Releases sind. Ein Architektur Branch kann theoretisch auch paralell über mehrere Software Releases bearbeitet werden. Das Tool muss folglich in der Lage sein, mit einem weiteren "Develop Branch" (oder mehreren) umzugehen und die Lebzeit dieses Branches mit einzubeziehen. 

## Users
Bei einem Pull Request soll das Tool nur die Befunde von "zugelassenen" Usern berücksichtigen, welche für Ausführung des Reviews genügend qualifiziert sind.

# Resultierende Anforderung an den (Review) Prozess
## Branchname
Bei einem Pull Request wird jeweils ein Feature/ Bugfix/ Hotfix Branch in den Develop / Release / Master (/Architektur) Branch gemerged. Um die Pull Requests unterscheiden zu können, müssen die Branches über ihren Branch Namen eindeutig identifizierbar sein. Der Name der Branches muss jeweils mit dem entsprechenden Prefix beginnen:

- feature_xyz
- bugfix_xyz
- hotfix_xyz
- release_xyz
- master
- develop
- develop2_architecture
- develop3_xyz
 




