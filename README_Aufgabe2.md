# Gruppenteilnehmer:
- 30252910
- 30245851
- 30248849

# Vorgehensweise
- Erstellung von Testfällen für das Frontened
- Erstellung von Testfällen für das Backend
- Anlegen der Github-Action
- Test der Github-Action (Wird sie bei einem Pull-Request ausgeführt?)
- Anlegen eines Sonarqube-Projekts
- Integration von Sonarqube in die Github-Action (Einfügen von build.yml)

# gewählte Lösungen
- Die Testfälle für das Frontend wurden in der todo_spec.js-Datei im Pfad cypress/integration erstellt. Dabei wurde auf eine möglichst hohe Abdeckung der Anwendung geachtet.
- Die bereits vorhandenen Testfälle für das Backend wurden etwas erweitert, um die Funktionalität tiefer zu testen.
- Die Github-Action wurde mit Hilfe der Dokumentation angelegt und mittels Push und Pull an/von das Repository getestet. Diese wurde auch ausgeführt, allerdings schlug sie aufgrund nicht erfolgreicher Tests fehl.
- Das Sonarqube-Projekt wurde ausgewählt und erstellt. Es wurden die entsprechenden Secrets eingetragen und die build.yml-Datei erstellt. In der Übersichtsseite der Github-Actions konnte nachvollzogen werden, dass die Analyse ausgeführt wird.


# Probleme - Lösungen und Erklärungen

## Korrektur der Anwendung
### GET /todos (unauthorisiert)
- erwartet wird Code 401, es wird aber 200 zurückgegeben
- Methode zur Dummy-Authentifizierung war nicht implementiert, wenn kein Token bereitgestellt wird
## Post /todos
- Der Test für unvollständige Todo-Einträge schlug fehl, da die API einen 201-Statuscode anstatt des erwarteten 400-Statuscodes zurückgab.
- Die POST /todos Route wurde um eine Validierung erweitert, die unvollständige oder ungültige Todo-Einträge abfängt und einen 400 Bad Request Statuscode zurückgibt.

## Github-Action
- Die Github-Action wurde zuerst nicht korrekt ausgeführt
- in der yaml-Datei wurden Syntax und Formatierungsfehler gemacht, wodurch die korrekte Ausführung verhindert wurde
- nach Korrektur der Fehler wurde die Action korrekt ausgeführt, auch wenn sie aufgrund von Fehler in den Tests fehlschlug

## Sonarqube
Die Integration von Sonarqube war leider nicht möglich. Bei der Erstellung des Projektes ist nur Zugriff auf Repositories der FH möglich, daher wurde ein lokales Projekt erstellt. Die entsprechenden Secrets wurden dann erstellt und im Repository eingegeben. Leider war trotzdem kein Zugriff auf das Repository möglich. Daher erfolgte eine Recherche, bei der ein Transfer des Repositories in die FH-SWF-Organisation vorgeschlagen wurde. Dies wurde entsprechend vollzogen und das Projekt konnte bei Sonarqube als Projekt angelegt werden. Leider stellte sich dabei heraus, dass es nach dem Transfer nicht mehr möglich war, die Einstellungen des Repositories zu erreichen, wodurch die entsprechenden Secrets nicht eingetragen werden konnten. 

Um dieses Problem zu umgehen wurde ein neuer Fork von dem ursprünglich erstellten Repository "todo-restless-effort" erstellt. Dieses wurde direkt unter der Organisation FH-SWF erstellt, womit Zugriff auf die Settings und damit die Secrets für Sonarqube ermöglicht wurde. Dabei handelt es sich um das jetzige Repository "swq_tb_jw_js".

# Ergebnisse der automatisierten Tests und SonarQube-Analysen

## Tests
Abschließend eine Übersicht über die vorhandenen Tests. Leider konnten für die fehlgeschlagenen Tests keine Lösungen gefunden werden. Hauptsächlich waren Validierungsregeln von den Tests betroffen, die nach unserert Meinung korrekt implementiert sein sollten, aber trotz dessen nicht korrekt ausgeführt bzw. erfolgreich getestet werden konnten.

| Test Description | Result | Time (ms) |
|------------------|--------|-----------|
| GET /todos (unautorisiert): sollte einen 401-Fehler zurückgeben, wenn kein Token bereitgestellt wird | ✓ | 11 |
| GET /todos: sollte alle Todos abrufen | ✓ | 14 |
| POST /todos: sollte ein neues Todo erstellen | ✓ | 17 |
| POST /todos: sollte einen 400-Fehler zurückgeben, wenn das Todo unvollständig ist | ✓ | 4 |
| POST /todos: sollte einen 400-Fehler zurückgeben, wenn das Todo nicht valide ist | ✕ | 7 |
| GET /todos/:id: sollte ein Todo abrufen | ✓ | 8 |
| GET /todos/:id: sollte einen 404-Fehler zurückgeben, wenn das Todo nicht gefunden wurde | ✓ | 14 |
| PUT /todos/:id: sollte ein Todo aktualisieren | ✓ | 9 |
| DELETE /todos/:id: sollte ein Todo löschen | ✓ | 12 |
| POST /todos (erweiterte Validierung): sollte einen 400-Fehler zurückgeben, wenn der Titel zu kurz ist | ✕ | 3 |
| POST /todos (erweiterte Validierung): sollte einen 400-Fehler zurückgeben, wenn das Datum ungültig ist | ✕ | 3 |
| POST /todos (Grenzwerte): sollte ein Todo mit maximal erlaubter Titellänge erstellen | ✓ | 5 |
| POST /todos (Grenzwerte): sollte einen 400-Fehler zurückgeben, wenn der Status außerhalb des gültigen Bereichs liegt | ✕ | 3 |

## Sonarqube
Sonarqube meldete zwei Security Hotspots. Der erste mit der Priorität "high" bezieht sich auf die einprogrammierten Credentials für die Keycloak-Anmeldung. Da es sich bei der Anwendung um eine Anwendung zum Testen und Lernen handelt und damit die Handhabung vereinfacht wird, wurde die Meldung bestätigt. Die Anwendung wird nicht produktiv genutzt.
Der zweite Hotspot bezieht sich auf die Offenlegung der Versionsnummer durch die Expressanwendung. Hiermit werden potenziell Angriffe erleichtert, da Schwachstellen der bestimmten Version genutzt werden könnten. Auch hier wurde das Risiko durch die Nutzung als nicht produktive Anwendung akzeptiert und bestätigt.
