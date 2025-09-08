fuer ein primary sample:

mache ein master sample.

```
MasterSampleDataObject master = new MasterSampleDataObject();
```

gib ihm eine id.  damit kannst du ein neues sample machen oder ein
bestehendes updaten.

```
List<IDContainerDataObject> idContainers = new ArrayList<>();
idContainers.add(new IDContainerDataObject(sampleId, "SAMPLEID"));
```

der string "SAMPLEID" ist ein id container typ aus cxx >
administration > stammdaten > system > id-arten.

hefte die id container an das sample.

```
master.setIdContainerDataObjects(idContainers);
```

das sample braucht zum speichern mindestens ein reposition date und einen
restamount.

```
master.setRestAmount(new AmountDataObject(new BigDecimal(1), AmountUnitWF.PC));
master.setRepositionDate(new Date());
```

AmountUnitWF.PC steht fuer 'piece'.

du haengst vermutlich noch mehr werte an das sample, ein beispiel ist
in buildSampleDataObject() in nsn_sampleregistry.ExtraInfoDialog oder
in buildSampleDataObject() in num_biomaterialregistry.RegistryDialog.

wenn du das sample direkt speicherst, mach es so:

```
this.workflowServiceDispatcher.getSaveSampleService().saveSamples(List.of(master),
taskContent);
```

wenn du das sample ueber einen patienten speicherst, mach es so:

```
patient.setSampleDataObjects(List.of(master));
this.workflowServiceDispatcher.getSavePatientService().savePatients(List.of(patientData), taskContent);
```

den task content gibt es mit:

```
new TaskContent(processInstanceId, taskId, processSessionId);
```

fertig.


fuer ein aliquot.

mache ein primary sample nur mit id-container an dem das aliquot
haengen soll.

```
MasterSampleDataObject master = new MasterSampleDataObject();
master.addIdContainerDataObject(new IDContainerDataObject(masterId, "SAMPLEID"));
```

mache ein derived sample fuer das aliquot.

```
DerivedSampleDataObject aliquot = new DerivedSampleDataObject();
```

gib dem aliquot eine id:

```
aliquot.addIdContainerDataObject(new IDContainerDataObject(aliquotId,
"SAMPLEID"));
```

tu das aliquot in eine map, um es am master zu speichern.  durch die
map werden die aliquot nach material-batches geordnet.  wir legen
unser eines aliquot an die position 0.

```
Map<Integer, List<AbstractSampleDataObject>> aliquotMap = new HashMap<>();
aliquotMap.put(0, new ArrayList<>(List.of(aliquot)));
```

hefte die aliquot map an das master.

```
master.setDerivedSampleMap(aliquotMap);
```

wenn es das master schon gibt, setze update auf true (ist das
noetig?).

```
master.setUpdate(true);
```

speicher das master.

```
this.workflowServiceDispatcher.getSaveSampleService().saveSamples(List.of(master),
taskContent); 
```

fertig.
