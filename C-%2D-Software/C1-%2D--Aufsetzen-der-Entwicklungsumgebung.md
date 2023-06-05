# Software-Entwicklungsumgebung

## Visual Studio

Microsoft Visual Studio Community 2022
Version 17.4.4
VisualStudio.17.Release/17.4.4+33213.308
Microsoft .NET Framework
Version 4.8.04084

Installed Version: Community

Visual C++ 2022   00482-90000-00000-AA061
Microsoft Visual C++ 2022

ASP.NET and Web Tools   17.4.326.54890
ASP.NET and Web Tools

Azure App Service Tools v3.0.0   17.4.326.54890
Azure App Service Tools v3.0.0

C# Tools   4.4.0-6.22608.27+af1e46ad38d900023f8b1a2839484e471ece1502
C# components used in the IDE. Depending on your project type and settings, a different version of the compiler may be used.

Microsoft JVM Debugger   1.0
Provides support for connecting the Visual Studio debugger to JDWP compatible Java Virtual Machines

NuGet Package Manager   6.4.0
NuGet Package Manager in Visual Studio. For more information about NuGet, visit https://docs.nuget.org/

TypeScript Tools   17.0.10921.2001
TypeScript Tools for Microsoft Visual Studio

Visual Basic Tools   4.4.0-6.22608.27+af1e46ad38d900023f8b1a2839484e471ece1502
Visual Basic components used in the IDE. Depending on your project type and settings, a different version of the compiler may be used.

Visual F# Tools   17.4.0-beta.22512.4+525d5109e389341bb90b144c24e2ad1ceec91e7b
Microsoft Visual F# Tools

Visual Studio IntelliCode   2.2
AI-assisted development for Visual Studio.

## Verwendete Versionen und Libraries

.NET 6.0

Nuget-Packages:
![image.png](/.attachments/image-ad7ba24f-1002-4bf2-aaca-abad9d97e392.png)

## Zusätzliche Tools
- Code Coverage
Hier hat sich die Fine Code Coverage Library als guter Kandidat herausgestellt. Die Installation geht sehr einfach, siehe https://marketplace.visualstudio.com/items?itemName=FortuneNgwenya.FineCodeCoverage2022. Visual Studio sollte geschlossen, werden, dann kann die Installation gemacht werden. Nach der Installation ist dann unter "View, Other Windows, Fine Code Coverage" die Übersicht über die Codeabdeckung ersichtlich. Die Abdeckung soll als Hilfsmittel dienen, um "Hotspots" zu identifizieren und einen Überblick über die Testabdeckung des Projektes zu erhalten. Sie soll nicht automatisiert werden (im Sinne eines Abbruchs oder Fehlers der Pipeline, wenn Kriterien nicht erfüllt werden o.ä.).
- Statische Codeanalyse
Ist schon in VS2022 integriert (Rechtsklick auf Projekt, Analyze and Code Cleanup...)

## Richtlinien
- Für Coding Guidelines sollen allgemein anerkannte "best practices" eingehalten werden, wie sie z.B. auf https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions oder auf https://www.freecodecamp.org/news/coding-best-practices-in-c-sharp/ aufgeführt werden.


# Continuous Deployment Umgebung
Für das Deployment wird gegenwärtig eine Azure DevOps Pipeline verwendet. Diese wird in einem .yml File definiert. Der Link zum File : https://dev.azure.com/michaelbos0816/_git/Aestimo?path=/azure-pipelines.yml.

Folgende manuellen Anpassungen wurden gemacht :
- trigger : Definiert, wann die Pipeline startet. Hier war zu Beginn 'main' eingetragen, wir wollen die Pipeline aber bei jedem Push starten, daher wird hier neu '*' eingetragen.
- VSBuild@1 : Ausgabe der Build-Artefakte in definiertes Verzeichnis. Hinzufügen von '/p:OutDir="$(Build.BinariesDirectory)"' zu dem msbuild Parametern.
- VisualStudioTestPlatformInstaller@1 : Es war notwendig, diese neue Stage hinzuzufügen, damit die Tests korrekt durchgeführt werden (sonst läuft die Pipeline nicht durch).
- ArchiveFiles@2 : Erzeugt ein Archiv vom Build-Output.
- PublishBuildArtifacts@1 : Erzeugt den Link für den Download des Build-Archives.

Gegenwärtig wird noch nicht zwischen verschiedenen Build-Konfigurationen unterschieden (z.B. Debug oder Release) und alle Artefakte (.exe, .dll, Zusatzfiles) werden aus dem Build-Output Verzeichnis gezippt und stehen nach einem erfolgreichen Build als Downloadlink zur Verfügung. Der Link ist hier zu finden :

![image.png](/.attachments/image-4eb40f3b-ab2a-4094-95fa-6b1b810c1535.png)

# Aestimo als Kommandozeilenvariante unter Linux (Ubuntu)

Die Erhebung der Anforderungen mit dem PM und der Entwickler hat zutage gebracht, dass doch eine Kommandozeilen-Variante des Tools sinnvoll wäre. Da wir das WPF Framework ausgewählt haben, ergibt sich hier ein kleines Problem, da das Framework (ohne grössere Aufwände) nur auf Windows läuft.

Was aber möglich ist, und auch schon verifiziert wurde, wäre die zusätzliche Implementation einer Kommandozeilen-Variante des Tools. Bei der Variante müssten die beiden unteren Layer (Data Access Layer und Business Logic Layer) 1:1 identisch sein, und nur der User Interface Layer wäre je nach Variante unterschiedlich.

Zu dem Zweck wurde ein Versuch mit einer Ubuntu VM Version 22.04 gestartet:
- In der Visual Studio Solution ein neues Projekt "AestimoCommander" erstellt und testweise einfach die Inhalte der beiden "common" Layer kopiert (nur Cloud Anteile, Server Variante wurde nicht getestet). Einige kleinere Anpassungen wurden gemacht:
  - Logging Framework wurde ausgelassen, anstelle wurde dem ILogger Interface ein ConsoleLogger, der einfach "WriteLine" macht übergeben (hier hat sich nun das zusätzliche ILogger Interface schon bewährt!)
  - Anstelle des MainWindow gibt es ein Program.cs mit einer main-Routine, die grob gesagt dasselbe tut wie ihr "grosser Bruder" (Aestimo mit WPF).
- Kompilieren muss man im Visual Studio manuell über die Command Line, zudem muss man im .csproj die "TargetRuntime" auf **_'<RuntimeIdentifiers>ubuntu.22.04-x64</RuntimeIdentifiers>'_** setzen. Die Kompilation macht man mit **_'dotnet build AestimoCommander.csproj --self-contained --runtime ubuntu.22.04-x64'_**. 
- Anschliessend kann man den Output im bin-Verzeichnis (inklusive der vielen .dll) in ein .7z Archiv packen, auf die VM kopieren und laufenlassen.

![image.png](/.attachments/image-afc154e5-db40-49f2-97af-b57659f15173.png)
![image.png](/.attachments/image-27afe4f9-791a-45dc-9656-6b8c8a2d131c.png)
