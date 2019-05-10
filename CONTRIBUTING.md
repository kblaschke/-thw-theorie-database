# Helfen, den Fragenkatalog zu verbessern
Jeder Freiwillige ist herzlich dazu eingeladen, den Fragenkatalog mit zu
verbessern. Um das Repository konsistent zu halten und den Prozess für alle
transparent zu gestalten, führe ich hier ein paar Hinweise auf, wie man dazu
beitragen kann.

## Nutze Git
Um alle Änderungen nachvollziehbar und überprüfbar zu machen, ist es
empfehlenswert, diese über den üblichen Git-Workflow einzupflegen. Dieser sieht
so aus:

- Erstelle einen Account [in GitLab](https://gitlab.kb-dev.net/).
- Forke dieses Repository in deinen Account. Diese Aktion legt eine persönliche
  Kopie des gesamten Git-Repositories an.
- Erstelle einen Branch, dessen Name deine Änderungen widerspiegelt (z. B.
  "fragen20xx" oder "fehlerkorrekturen"). Der Branch-Name muss lediglich
  eindeutig sein, und wird nach Merge in master wieder gelöscht.
- Nimm die nötigen Änderungen vor, entweder direkt in GitLab oder über eine
  lokale Kopie, die du mit "git clone" erzeugt hast.
- Wenn du fertig bist, erstelle im
  [Haupt-Repository](https://gitlab.kb-dev.net/kblaschke/thw-theorie-database)
  einen Merge-Request.
- Nach erfolgreichen Review und eventuellen Nachbesserungen wird der Branch
  gepullt und in den master-Branch gemergt.

## Git ist zu kompliziert?
Falls du mit Git nicht vertraut bist, besteht die Möglichkeit, die
entsprechenden Dateien direkt aus GitLab herunterzuladen, zu bearbeiten und dann
per E-Mail an kai.blaschke@kb-dev.net zu senden. Ich übernehme dann den oben
genannten Workflow, und pflege die Änderungen unter deinem Namen ein, falls
gewünscht .

## Datenbank-Dump bearbeiten
Der SQL-Dump in diesem Repository enthält alle Fragenkataloge seit 2007. Die
älteren Versionen sind lediglich historisch, auf der Website werden in der Regel
nur die aktuellsten beiden Versionen verwendet (und auch nur, wenn der Katalog
kürzlich aktualisiert wurde). Beachte also bei der Bearbeitung, aktualisierte
Fragenkataloge mit einem neuen Jahr hinzuzufügen, anstatt den letzten zu
editieren und die Jahreszahl zu ändern. Unterjährige Aktualisierungen, die
meistens kleinere Änderungsdienste sind, können am bestehenden Katalog
vorgenommen werden.

Zum Bearbeiten empfehle ich, den SQL-Dump in einen MySQL-Server zu importieren
und mit einer Benutzeroberfläche wie MySQL Workbench oder phpMyAdmin zu
bearbeiten. Diese Werkzeuge haben meist auch eine Export-Möglichkeit für
Dump.

Du kannst nach dem Import mit diesen SQL-Statements den Katalog vollständig
kopieren, dazu <altes Jahr> und <neues Jahr> ersetzen:

    INSERT INTO `abschnitte` SELECT `Nr`, <neues Jahr> AS `Jahr`, `Beschreibung` FROM `abschnitte` WHERE `Jahr` = <altes Jahr>;
    INSERT INTO `fragen` (`Jahr`, `Abschnitt`, `Nr`, `Frage`, `Antwort1`, `Antwort2`, `Antwort3`, `Loesung`) SELECT <neues Jahr> AS `Jahr`, `Abschnitt`, `Nr`, `Frage`, `Antwort1`, `Antwort2`, `Antwort3`, `Loesung` FROM `fragen` WHERE `Jahr` = <altes Jahr>;

Der Primärschlüssel "ID" in der Tabelle "fragen" wird automatisch erzeugt, und
muss nicht fortlaufend sein.

## Datenbank-Dump exportieren
Achte bitte beim Export darauf, dass du das gleiche Format benutzt wie bisher.
Insbesondere die Zeichenkodierung muss passen (UTF-8), sonst gehen Umlaute
kaputt. INSERTs sollten als ein Statement pro Datensatz exportiert werden.
Das ist zwar langsamer, erleichtert aber die direkte Bearbeitung der Datei
oder das Kopieren einzelner Statements.

Am einfachsten ist es, wenn du nach erfolgter Bearbeitung die Unterschiede von
Git anzeigen lässt. Zeigt das Diff nur deine Änderungen, ist es okay. Zeigt das
Diff an, dass nahezu alles in der Datei geändert wurde, solltest du einen neuen
Dump mit angepassten Optionen erstellen, oder deine Änderungen (die neuen
Inserts) manuell in den bestehenden Dump einfügen.

Abseits von Git gibt es andere Vergleichswerkzeuge wie
[WinMerge](http://winmerge.org) oder "diff" unter Linux, um die Änderungen zu
prüfen.

## Nennung unter "Beitragende"
Grundsätzlich werden alle Autoren, die bei der Bearbeitung mitgeholfen haben,
namentlich erwähnt. Du hast natürlich die Option, ein Pseudonym statt deinem
realen Namen zu verwenden oder den Beitrag anonym zu halten. Wenn du Git
benutzt, kannst du das über den Commit selber steuern. Wenn du mir die Dateien
per Mail schickst, schreib bitte dazu, in welcher Form du genannt werden
möchtest.