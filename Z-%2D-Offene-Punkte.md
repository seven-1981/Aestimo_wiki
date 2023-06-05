- Gibt es ein OST Template für die Dokumentation/ den Technischen Bericht?
  - Ja gibt es. Noch offen.
- Ist es OK, die App zum Technischen Durchstich für die Zwischenpräsentation im Juni zu nehmen?
  - Prinzipiell ja, aber die App ist ein Wegwerfprodukt. Wir müssen darauf achten, dass es keine Doppelspurigkeiten gibt, die zu Mehraufwand führen.
- Gitlab API ist zu wenig umfangreich, um es für unsere Anwendung anzuwenden
  - Begründung im Bericht nicht vergessen (warum wird es nicht angewendet)

- Soll Aestimo einen Persistenz-Layer bekommen? Konkreter: Soll man Reports "speichern" und "laden" können? Oder stellen wir uns auf den Standpunkt, dass das Repository schon den Persistenz-Layer darstellt?
  - Der Output wird ein nicht änderbares PDF sein, welches auf dem SVN abgelegt wird. Auf dem SVN kann das Dokument aufgrund der Historie immer wieder geholt werden. Deshalb braucht das Tool selber keinen Persistenz-Layer

- Ist eine WPF Applikation mit VS2022 auf Linux portierbar? Stichwort Automation (CI Pipeline über Jenkins auf Linux Server, macht ein Aestimo CLI Sinn?)
  - Wird eher schwierig, aber eine Konsolenapplikation ist machbar und wurde verifiziert (auf Ubuntu 22.04).

- Gibt es eine Möglichkeit, die Jira-Story zu extrahieren vom Bitbucket API ?
  - Ja, die gibt es, für die Server Variante kann man über die Jira Integration die Tickets die einem Pull Request zugeordnet sind, abfragen. Bei der Cloud Variante geht es scheinbar nicht direkt.