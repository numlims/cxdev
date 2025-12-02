# im prod

## bei rollback 'automatisch starten' aktivieren

wenn ein workflow geloescht wird faellt er auf die version davor zurueck.  diese mit play-button starten und 'workflow automatisch starten' aktivieren.


## ohne neuen parameter

workflow version in bpmn hochsetzen.

workflow bauen und hochladen (in workflow-prozesse, oberes fenster).

den workflow dort mit play button starten, und in
der gruppen-inbox (workflow > workflow-aufgaben) die alte instanz
stoppen.


## mit neuem parameter

wenn ein neuer parameter in `parameters.xml` dazugekommen ist:

die zeile `<exclude>parameters.xml</exclude>` in `src/assembly/<workflow name>.xml` auskommentieren, damit die neue parameters.xml uebernommen wird.

workflow version im bpmn hochsetzen, evtl. dort auch 'dev' aus dem namen loeschen, dass der name gleich ist wie beim aktuell hochgeladenen workflow.

workflow bauen und hochladen (in workflow-prozesse, oberes fenster).

evtl. parameter neu setzen.

starten mit play button.

alte version stoppen in workflow-aufgaben > gruppen-inbox.

## parameter angepasst ohne neuen upload

wurde in centrax nur der wert von einem parameter geaendert, ohne dass der 
workflow neu hochgeladen wurde, dann nicht den play button in workflow-prozesse klicken, sondern in der
gruppen-inbox (workflow > workflow-aufgaben) **zwei** mal play
klicken, damit die aktualisierten einstellungen uebernommen werden.



# im dev

die workflow-version von einem neuen upload darf nicht gleich der bestehenden version sein, von daher die alte version loeschen oder vor jedem neuen upload die version aendern.

darauf achten, dass es z.b. bei nsn_restregistry eine extra dev version gibt, damit nutzer mit der nicht-dev version testen koennen.




