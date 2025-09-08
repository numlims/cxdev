in parameters.xml eine neue zeile machen:

```
<parameter name="storageConditions" type="java.lang.String"
value="-20 Degrees;-7 Degrees;Room Temperature" />
```

in sampleregistry.bpmn 'storageConditions' zur local variable list
fuer den gesamten prozess hinzufuegen.

dann 'storageConditions' ins input data mapping der extra_info
aktivity hinzufuegen:  from data item storageConditions to name
storageConditions und data type java.lang.String.

in ExtraInfoDialog.java in readInputTaskVariables() die
storageConditions in eine liste speichern:

```
storageConditions = List.of(((String)
fromInputTaskVars("storageConditions")).split(";"));
```

diese liste in postStorageConditionList an die samples schreiben:

```
sample.setStorageConditionList(storageConditions);
```

postStorageConditionList in getViewContent() fuer die samples
aufrufen:

```
postStorageConditionList(basisSamples);
```

in der klasse fuer die tabellen zeile, ExtraInfoRow, die storage
conditions in ihr feld schreiben:

```
WorkflowUtils.setValueToComboBoxByString(this.storageConditionBox, 
sample.getStorageConditionList(), null);
```

fertig.