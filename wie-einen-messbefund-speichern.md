die messparameter in centraxx anlegen:  administration > stammdaten >
messwerte > messparameter.  die codes der messparameter merken.

das messprofil in centraxx anlegen und die neuen messparameter
hineintun:  administration > stammdaten >
messwerte > messprofil.  den code vom messprofil merken.

ein neues finding machen:

```
MeasurementFindingDataObject finding = new
MeasurementFindingDataObject();
```

wenn ein bestehendes finding geupdatet werden soll, das finden mit:

```
this.workflowServiceDispatcher.getLoadLaborFindingDataService().findFindingByPatientAndProfil(patient, messProfilCode);
```

wenn das finding neu ist, name, datum und messprofil-code setzen:

```
finding.setShortname("my_new_finding")
finding.setProfilCode(messProfilCode);
finding.setFindingPrecisionDate(new DatePrecisionDataObject(new
Date(), DatePrecisionWF.EXACT));
finding.setFindingType(FindingType.SAMPLE);
```

fuer die messparameter eine liste findingValues machen.

```
List<FindingValueDataObject> findingValues = new ArrayList<>();
```

die messparameter werte in diese liste legen.

```
DataObjectUtils.buildFindingValue(findingValues, IS_REST, true);
```

die values an das finding heften.

```
finding.setFindingValueDataObjects(findingValues);
```

das finding an sein sample heften.

```
sample.setMeasurementFindingDataObjects(List.of(finding));
```