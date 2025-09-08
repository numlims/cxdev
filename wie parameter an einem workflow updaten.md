die parameters.xml wird in assembly/<workflow name>.xml excluded,
damit ihre werte, z.b. hochgeladene files, im u.i. persistiert werden.

aendert man etwas an der parameters.xml, dann:

- parameters.xml nicht mehr excluden.

- workflow bauen und hochladen.

- evtl. zurueckgesetzte parameter (z.b. files) im ui neu setzen
(schraubenschluessel symbol).

- parameters.xml wieder excluden.

- die workflow version im bpmn um eins hoch setzen: (hier `<bpmn2:process id="my_workflow_v1" ...` und hier `<bpmndi:BPMNPlane id="BPMNPlane_Process_1" bpmnElement="my_workflow_v1">`)

- workflow wieder bauen und hochladen, ohne wie sonst die vorherige
version zu loeschen, damit die neu eingegebenen parameter bleiben.
das hochladen ohne vorher loeschen geht, weil die workflow version jetzt anders ist.
