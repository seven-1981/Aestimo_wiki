## Ziel des technischen Durchstichs
Die Zielsetzung des technischen Durchstichs, ist es, eine lauffähige Applikation zu implementieren, bei der eine Verbindung senkrecht durch alle verwendeten Technologien und Layer ermöglicht wird. Die Applikation soll von einem Dummy-Repository auf Bitbucket einen oder mehrere beliebige Kommentare über das REST API abfragen, in eine entsprechende Objektstruktur in der Applikation abfüllen und die Daten daraus in einem PDF Dokument darstellen.

Zudem sollen verschiedene Technologien und Libraries ausprobiert werden, so dass wir uns zu Beginn der "heissen" Phase dann festlegen können.

## Bisher verwendete Libraries und Technologien
- Bitbucket REST API (HTTP Request)
Bitbucket dient als Code-Hosting Plattform und besitzt ein REST API, über welches unter vielen anderen Funktionalitäten gemachte Kommentare abgefragt werden können. Die in der Response zurückgelieferten Daten werden in JSON zurückgegeben. Bitbucket stellt ihr API als Swagger / OpenAPI zur Verfügung: https://api.bitbucket.org/swagger.json. Auf z.B. https://editor.swagger.io/ kann der zurückgegebene JSON Text einkopiert und die API Definitionen angeschaut werden, was sehr hilfreich ist.

- NSwag Library
NSwag bietet ein Framework für das Handling mit Open API Spezifikationen. Es können z.B. selbst Open API Dokumente erstellt werden, oder, was in unserem Fall interessanter ist, Client HTTP Code für C# erstellt werden. Das Framework kann die Open API Spec. einlesen und den Code daraus erzeugen. Man muss dann nicht mühsam alle DTOs und Client-Funktionen (HTTP-Requests) selber schreiben.

- Refit Library
Die Library haben wir im Kurs Mobile App Engineering kennengelernt. Sie dient als "REST API Consumer". Es lassen sich sehr bequem Interfaces für die HTTP Queries definieren. Die HTTP Requests werden dann ausgeführt und die erhaltenen Daten werden dann in eine definierbare DTO Objektstruktur abgefüllt. Der Nachteil der Library gegenüber NSwag ist, dass es scheinbar keine direkte Möglichkeit gibt, die Open API Spezifikation einzulesen, sondern man muss die DTOs aus dem Model und die Request-Funktionen selbst implementieren.

- JSON Parser (Newtonsoft)
Library für das Parsen und Serialisieren von JSON. Die in der HTTP Response erhaltenen JSON-Daten werden mit Hilfe der Library in Objekte umgewandelt. Die Refit Library kann einen beliebigen JSON Parser verwenden. Über die Klasse "RefitSettings" kann also auch eine Newtonsoft JSON Parser Instanz übergeben werden. Auch die NSwag Library kommt gut mit der Newtonsoft Library klar (Vermutung: Sie greift sogar darauf zurück - Library Dependency).

- WPF Applikation
Mit dem Windows Presentation Foundadion Framework wird das GUI erstellt. Beim technischen Durchstich besteht es nur aus einem Window, mit einem Eingabefeld für die REST URL resp. Buttons für das Ausführen der Aktionen.

- MigraDoc Library
Mit der MigraDoc Library können PDF-Dokumente erstellt werden. Die Inhalte können relativ flexibel implementiert werden, d.h. denkbar wäre z.B. die Dokumentstruktur in eine Klassenstruktur in der Software abzubilden (Composite Pattern, Visitor Pattern).

- NLog Libary
Die NLog Library ist eine sehr flexible open-source Logging Bibliothek für .NET. Die Integration in die Applikation für den Technischen Durchstich ging sehr unkompliziert. Es lassen sich mit wenigen Zeilen Code Logmeldungen ausgeben resp. Logmeldungen in eine Datei umleiten. Dadurch ist die Library ein sehr guter Kandidat für die Masterarbeit. Siehe https://github.com/nlog/nlog/wiki/Tutorial.

- Handlebars.NET
Framework für die Generierung von HTML oder XML. Zusatzoption zu MigraDoc, da so eine zweite Möglichkeit für die Generierung der Dokumente existieren würde. Die HTML oder XML Erzeugnisse könnten dann auch in PDFs konvertiert werden. Integration in Applikation lief problemlos. Valabler Kandidat für das Produzieren eines "intermediate" Output...

- RestSharp
Einfacher REST API Client - noch nicht ausprobiert. Siehe https://restsharp.dev/intro.html#basic-usage.

- Ninject
Dependency Injection Framework - funktioniert gut, besser als von Hand einen "Service Locator" zu implementieren. Siehe http://www.ninject.org/.

- Avalonia
Alternative zu WPF - noch nicht ausprobiert. Falls in Linux auch ein GUI gefordert würde. Siehe https://avaloniaui.net/.

- Automapper
Für Data Binding - noch nicht ausprobiert. Siehe https://docs.automapper.org/en/stable/index.html.

### Weitere ausprobierte Libraries
Die hier aufgeführten Libraries wurden ausprobiert, schieden jedoch aus diversen Gründen wieder aus.
- Swashbuckle
Ähnlich wie NSwag, Integration hat aber nicht so gut geklappt wie NSwag
- Postsharp
Sehr mächtiges Framework, gibt auch Logging Möglichkeit als AOP (aus Java Kurs bekannt). Mit aspektorientierter Programmierung liesse sich Logging vom Code trennen. Einbindung hat zwar geklappt, aber das Framework ist leider nicht kostenfrei, daher scheidet es aus.


## Repository + CI/CD auf Microsoft Azure DevOps
https://dev.azure.com/danielschlatter/Aestimo_TechBreakthrough

Aufsetzen der Pipeline mit YAML: https://dev.azure.com/danielschlatter/Aestimo_TechBreakthrough/_git/TechBreakthrough?path=/azure-pipelines.yml

### Problem beim Ausführen der Unit-Tests in der Pipeline mit NUnit
https://stackoverflow.com/questions/73404846/devops-vstest2-errorcould-not-find-testhost
https://github.com/microsoft/azure-pipelines-tasks/issues/15449
https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/file-matching-patterns?view=azure-devops
https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/reference/vstest-v2?view=azure-pipelines
Nachdem im .yaml File der Eintrag
- task: VisualStudioTestPlatformInstaller@1
  inputs:
    versionSelector: 'SpecificVersion'
    testPlatformVersion: '17.2.0'

ergänzt wurde, wurden die Tests korrekt ausgeführt.

## Beschreibung der Applikation
Die Applikation besteht aus einem einzelnen Window:

![image.png](/.attachments/image-506219af-c628-4023-aaa6-6605f26332bd.png)

Im Eingabefeld wird der relative URL Pfad zur benötigten Ressource angegeben. Im erstellten Bitbucket Repository gibt es genau einen Pull-Request (die nummeriert werden, daher die 1), mit einem Kommentar. Die Basis-URL ist 
https://api.bitbucket.org/2.0/repositories/tech-breakthrough/aestimo_breakthrough/

Der Pull-Request ist hier zu finden:
https://bitbucket.org/tech-breakthrough/aestimo_breakthrough/pull-requests/1

Nachdem der Button "Create PDF" geklickt wurde, setzt die Applikation die beiden URL-Teile zusammen, macht dann darauf einen GET-Request und erhält einen String in JSON Format, der dann mit dem JSON Parser dynamisch geparst wird, d.h. ohne Kenntnis der Datenstruktur (ohne DTOs). Dies ist zwar "einfacher", aber fehleranfälliger und schwerer nachvollziehbar. Daher gibt es noch den 2. Button "Use Retrofit", welcher dasselbe mit der Refit Library bewerkstelligt (nicht mehr bevorzugte Variante).

Die Applikation ist zusätzlich noch als "Console Application" konfiguriert, damit man die Logs in der Konsole anschauen kann (z.B. JSON-Response, Kommentar-Text "Hier ist ein Fehler!").

Schliesslich wird die Response in die DTOs abgefüllt und als Text in ein PDF abgefüllt. Dieses wird im Output-Verzeichnis der Applikation erzeugt.

Wichtig!
Die Authentisierung wird mit einem sogenannten "Bearer Token" gemacht (String). Dieses muss auf dem Bitbucket Server gelöst werden und dann als "HTTPS authentication header" mitgegeben.

Weitere Buttons, die weitere Funktionalitäten testen:
- "Parse OpenAPI": Generiert aus der als Textfile gespeicherten (und korrigierten) Open API Definition mit Hilfe der NSwag Library den HTML-Client Code.
- "Use NSWag": Macht einen Aufruf mit dem von NSwag generierten HTML-Client.
- "Gen HTML": Generiert mit dem über NSwag abgefragten Kommentar ein kleines Stück gültigen HTMLs und gibt es mit dem Logger aus (kann kopiert werden und als HTML-File abgespeichert werden - dies ist nicht implementiert).

## Tests
-Mit Applikation
-Mit Command Line Interface (curl)

## Aufsetzen Bitbucket Repository
Damit einige weiterführende Abfragen getätigt werden können, wurde das Bitbucket Repository gemäss folgender Grafik mit einigen Branches und Commits ergänzt (develop, release1, feature1, feature2):
![image.png](/.attachments/image-6623b31b-9613-4283-8c90-adfefd3dc969.png)

## Zugriff auf Bitbucket Repository
https://bitbucket.org/tech-breakthrough/workspace/overview
https://bitbucket.org/tech-breakthrough/aestimo_breakthrough/src/master/

https://help.livelyapps.com/pocketquery-for-jira-server/bitbucket-rest-api

## REST API Info Bitbucket
https://developer.atlassian.com/cloud/bitbucket/rest/intro/
https://developer.atlassian.com/cloud/bitbucket/getting-started/
https://developer.atlassian.com/cloud/bitbucket/rest/intro/#authentication
https://developer.atlassian.com/cloud/bitbucket/rest/api-group-pullrequests/

https://developer.atlassian.com/server/bitbucket/rest/v807/intro/#intro

https://support.atlassian.com/bitbucket-cloud/docs/using-access-tokens/

https://confluence.atlassian.com/bbkb/what-is-a-repository-slug-1168845069.html#:~:text=A%20repository%20slug%20is%20a,become%20%22%20foo%2Dbar%20%22.

### Probleme mit dem Bitbucket Open API
Die Open API Spezifikation von Bitbucket ist (vermutlich) nicht korrekt ??? Grund für die Vermutung: Kopiert man den JSON Text von https://api.bitbucket.org/swagger.json in den Online Editor, erscheint folgende Fehlermeldung: 
![image.png](/.attachments/image-e21f64c3-71f1-43a2-a45e-df911a56847c.png)
Damit hat dann auch die NSwag Library ein Problem, da beim Parsen genau an der Stelle ein Exception auftritt :( 
Lösung: In der monierten Definition "definitions.pipeline_schedule_post_request_body.allOf.1.properties" das "required" Feld gesamthaft entfernen. Die C# Applikation "Technical Breakthrough" liest aus diesem Grund auch nicht mit der Funktion "FromUrlAsync" direkt von der Bitbucket-Seite, sondern mit "FromFileAsync" die als Textfile kopierte Open API Spezifikation. So kann der Fehler korrigiert werden und das Parsen funktioniert korrekt.

Wie die Integration gemacht werden kann, ist z.B. hier zu finden:
https://blog.logrocket.com/generate-typescript-csharp-clients-nswag-api/

## Bitbucket Server REST API
Das Bitbucket Server REST API ist zum Cloud Bitbucket REST API unterschiedlich. Gemäss PM ist die eingesetzte Bitbucket Server Version 8.7.1. (siehe https://dac-static.atlassian.com/server/bitbucket/8.7.swagger.v3.json?_v=1.403.0). Auch dieses API weist diverse semantische und syntaktische Fehler auf.
- Gemäss Open API 3 dürfen DELETE Requests keinen Request Body haben (kommt aber leider vor)
- Einige "Path" Angaben sind ungültig (z.B. statt 'requestBody' steht 'requestbody', was zu Problemen führt)
- Diverse "operationId" sind laut Fehlermeldungen nicht eindeutig, was aber sein sollte

Durch diese Probleme spuckt das NSwag Framework dann auch nicht ganz lauffähigen Code (über 100'000 Zeilen) aus. Doch die Probleme konnten manuell korrigiert werden.
- Ersetzen von 'return default(void)' durch 'return'
- Umbenennen der diversen Funktionsnamen vom Typ '1PostAsync' in 'PostAsync1'

Scheinbar können die Funktionsnamen nicht immer korrekt aufgelöst werden. Glücklicherweise sind aber relativ ausführliche Kommentare enthalten, welche die Identifikation der Funktionen ermöglichen.

## GitLab REST API
Das GitLab REST API wird auf https://docs.gitlab.com/ee/api/rest/ veröffentlicht. Leider hat sich herausgestellt, dass es noch nicht genügend umfangreich ist, als dass wir es für die Arbeit einsetzen können. Konkret fehlen die Möglichkeiten, gewisse Ressourcen (Inhalte von Merge Requests, Kommentare) abzufragen.
Zudem war das veröffentlichte API nicht brauchbar.

Nur die Version 2 auf https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/api/openapi/openapi_v2.yaml hat funktioniert (Konvertierung mit https://www.convertjson.com/yaml-to-json.htm, zuerst auf Open API Version 3 konvertiert mit https://editor-next.swagger.io/).

Die Version 1 https://gitlab.com/gitlab-org/gitlab/-/blob/master/doc/api/openapi/openapi.yaml konnte nicht geparst werden (weder von swagger.io, von https://apitools.dev/swagger-parser/online/# noch von der NSwag Library).

Eine Möglichkeit mit GraphQL, einem anderen Query API gäbe es, doch dies würde weitere Frameworks und Mehraufwand mit sich ziehen. Zudem ist der Nutzen für uns beschränkt, da in der SEAG nur Bitbucket eingesetzt wird. 

Aus diesen Gründen werden wir wohl die Option "GitLab" vorerst streichen. Die Software sollte aber so designt werden, dass eine zukünftige Anbindung ermöglicht wird.

## Tests mit 'curl'
https://reqbin.com/req/c-haxm0xgr/curl-basic-auth-example

curl --request GET --url "https://api.bitbucket.org/2.0/repositories/tech-breakthrough/aestimo_breakthrough/pullrequests/1/comments" --header "Authorization: Bearer wcfOgiK3fG6JWUMOO4ri" --header "Accept: application/json"


## Refit Library
https://code-maze.com/using-refit-to-consume-apis-in-csharp/
https://codetraveler.io/2021/02/17/upgrading-to-refit-v6-0/
https://github.com/reactiveui/refit

## Newtonsoft JSON Library
https://www.newtonsoft.com/json
https://www.newtonsoft.com/json/help/html/DeserializeObject.htm

## MigraDoc Library
http://www.pdfsharp.net/MigraDocOverview.ashx
http://www.pdfsharp.net/wiki/HelloMigraDoc-sample.ashx
http://www.pdfsharp.net/wiki/MigraDocFirstSteps.ashx

https://damienbod.com/2018/10/03/creating-a-pdf-in-asp-net-core-using-migradoc-pdfsharp/


## Forumsbeiträge (Code-Review)
https://community.atlassian.com/t5/Bitbucket-questions/How-to-generate-code-review-report-in-bitbucket/qaq-p/637665

## C# Testing Frameworks
https://www.browserstack.com/guide/c-sharp-testing-frameworks

# Konfigurationsmanagement
- Die Version des Tools sollte im generierten Dokument sichtbar sein.
- Die Version des Dokuments sollte ebenfalls sichtbar sein (Struktur, Kapitel, Einleitung etc.)
- Zudem sollten die Hashes der git-Commits, mit denen das Review gemacht wird (Differenz), sichtbar sein (kommt vom Repository)
- Ev. eine automatische Build-Nummer einführen (siehe z.B. https://stackoverflow.com/questions/826777/how-to-have-an-auto-incrementing-version-number-visual-studio)

Die Tool-Version könnte z.B. wie folgt aussehen:
Version Major.Minor.Build
Major++ bei nicht mehr rückwärtskompatiblen Änderungen (des Tools), bei grundlegend neuen Features
Minor++ bei rückwärtskompatiblen Änderungen (z.B. REST API), bei kleineren neueren Features oder Anpassungen
Build-Nummer bei jedem Build geändert

## Management des REST API
Es ist damit zu rechnen, dass Bitbucket ihr API (Cloud und Server) während der Entwicklung des Tools ändern. Zudem ist geplant, dass Bitbucket bis 2024 ihr Angebot in die Cloud auslagert, weswegen die Firma dies ebenfalls tun muss.

Um die Anpassungen am API abzukapseln und die SW unabhängig davon zu designen, macht es Sinn, pro unterstütztem API einen Service-Adapter einzuführen. Dessen Aufgabe ist es, Änderungen an den APIs "abzufedern", so dass in weiter innen liegenden Software-Schichten keine Anpassungen mehr nötig sind. Ev. macht es Sinn, diesen Adaptern eine dem angepassten API entsprechende, eigene SW-Version zu geben.

Bei einem Update muss also
- die neue Open API Spezifikation heruntergeladen werden
- die Spezifikation muss validiert werden und ggf. müssen manuelle Anpassungen gemacht werden
- ggf. müssen die zugehörigen Service-Adapter aktualisiert werden.
- die Minor Nummer inkrementiert werden

Wichtig ist auch, dass die unterschiedlichen Versionen an einer Stelle zueinander in Relation gebracht werden. So kann man einfach feststellen, welche API Versionen mit welcher Tool- oder Dokumenten-Versionen zusammen funktionieren. Diese Gegenüberstellung soll im Tool ersichtlich sein (z.B. über das Menu) und ggf. kann sie auch im generierten Dokument eingefügt werden.