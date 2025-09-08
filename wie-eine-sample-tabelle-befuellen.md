du machst eine neue row klasse, die die ui felder enthaelt, die du am
sample edieren moechtest.

```
class MyRow extends SampleRow {
}
```

du machst fuer jedes ui-feld das in der row sein soll ein attribut.

```private DateTimeField samplingDateField;```

im konstruktor initialisierst du die felder.

```
public ExtraInfoRow() {
  super();

  this.samplingDateField = new DateTimeField();
}
```

der konstruktor ruft auch super() auf.

die geerbte methode getUIArray() gibt die felder zurueck in der
reihenfolge in der sie in der zeile landen.

```
public Object[] getUIArray() {
  return new Object[] { this.samplingDateField };
}
```

es gibt eine korrespondierende methode, fillTableHeader, die die namen
und klassen der einzelnen felder fuer die tabellen-kopfzeile setzt.
anscheinend braucht eine tabelle das um die felder darstellen zu
koennen.

```
public static void fillTableHeader(SampleTable<?> table) {
    table.addContainerProperty("sampling date", DateTimeField.class,
    null);
}
```

wenn ein neues feld dazu kommt muss es in getUIArray() und
fillTableHeader() aufgenommen werden (in fillTableHeader auch wenn die
reihenfolge der felder geaendert wird?)

die geerbte set methode setzt das sample fuer die row.

```
public void set(Sample sample) {
  this.sample = sample;
  
  Util.dateTimeToUI(this.samplingDateField, sample.getSamplingDate());
}
```

die geerbte updateSample methode schreibt eingegebene werte aus dem UI
ans sample.

```
public void updateSample() {
  sample.setSamplingDate(Util.dateTimeFromUI(samplingDateField));
}
```

spaeter wird das updaten fuer jede row durch myTable.updateSamples()
getriggert.

optional koennen felder einer validation list hinzugefuegt werden, im
moment durch ueberschreiben von einer getFieldsToValidate() method, in
zukunft (hoffentlich) durch aufrufen von
addMandatoryField(samplingDateField) im konstruktor.

die row waere soweit fertig.

du machst eine neue tabelle und uebergibst die MyRow klasse. 

```
SampleTable<MyRow> myTable = new SampleTable<MyRow>(null, MyRow::new);
```

du fuellst den table header.

```
MyRow.fillTableHeader(myTable);
```

der header sollte gefuellt werden bevor die samples gesetzt werden,
sonst rendert die tabelle anscheinend nicht.

du setzt die samples.

```
myTable.set(samples);
```

und fuegst die tabelle z.b. zu einem VerticalLayout hinzu.

```
layout.addComponent(myTable);
```

bevor du values aus den samples speichern moechtest, rufe updateSamples auf der
tabelle auf:

```
table.updateSamples();
```

dann kannst du an die angegebenen sampling dates kommen, z.b. so:

```
for (Sample sample : table.getSamples()) {
  LOG.info("date:" + sample.getSamplingDate());
}
```